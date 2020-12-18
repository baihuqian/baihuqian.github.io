---
layout: post
title: 'Lambda-based CDK Custom Resource with Input and Output'
date: '2020-12-17 20:10'
tags:
 - AWS
---

[AWS Cloud Development Kit (CDK)](https://aws.amazon.com/cdk/) is a great way to describe your infrastructure in a strongly-typed language. CDK provides support for many resources out of the gate, and also have integration with CloudFormation resources. However, there are still gaps in support, mainly due to

1. Gap in CloudFormation support by AWS services. CDK compiles code into a CloudFormation template so it cannot accomplish what CloudFormation does not support.
2. CDK and CloudFormation cannot manage resources created outside a stack, and sometimes it is impossible or too risky to recreate resources, such as DynamoDB tables.

Therefore, CloudFormation and CDK provides [Custom Resource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) that allows custom logic during stack orchestration.

This post discusses how to create a Lambda-function based CDK Custom Resource that takes input from the stack and produces output that can be used by the stack. As an illustration in this post, the custom resource enables DynamoDB stream on an existing table and returns the latest stream ARN so that the stack can create and subscribe a Lambda function to it. I'll use TypeScript for CDK and Python3 for Lambda functions.

## Via CloudFormation's Integration with Lambda
CloudFormation can invoke a Lambda function with the following request:
```json
{
   "RequestType" : "Create",
   "ResponseURL" : "http://pre-signed-S3-url-for-response",
   "StackId" : "arn:aws:cloudformation:us-west-2:123456789012:stack/stack-name/guid",
   "RequestId" : "unique id for this create request",
   "ResourceType" : "Custom::TestResource",
   "LogicalResourceId" : "MyTestResource",
   "ResourceProperties" : {
      "Name" : "Value",
      "List" : [ "1", "2", "3" ]
   }
}
```
The `"ResourceProperties"` are properties supplied to the CustomResource's constructor. The `"ResponseURL"` is a pre-signed URL that CloudFormation expects the Lambda to call with the outcome and result. CloudFormation provides the following [sample code](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-lambda-function-code-cfnresponsemodule.html#w2ab1c27c24c14b9c15):

```python
# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0

from __future__ import print_function
import urllib3
import json

SUCCESS = "SUCCESS"
FAILED = "FAILED"

http = urllib3.PoolManager()


def send(event, context, responseStatus, responseData, physicalResourceId=None, noEcho=False, reason=None):
    responseUrl = event['ResponseURL']

    print(responseUrl)

    responseBody = {
        'Status' : responseStatus,
        'Reason' : reason or "See the details in CloudWatch Log Stream: {}".format(context.log_stream_name),
        'PhysicalResourceId' : physicalResourceId or context.log_stream_name,
        'StackId' : event['StackId'],
        'RequestId' : event['RequestId'],
        'LogicalResourceId' : event['LogicalResourceId'],
        'NoEcho' : noEcho,
        'Data' : responseData
    }

    json_responseBody = json.dumps(responseBody)

    print("Response body:")
    print(json_responseBody)

    headers = {
        'content-type' : '',
        'content-length' : str(len(json_responseBody))
    }

    try:
        response = http.request('PUT', responseUrl, headers=headers, body=json_responseBody)
        print("Status code:", response.status)


    except Exception as e:

        print("send(..) failed executing http.request(..):", e)
```

For our use case, we will create a folder `custom-resource-handlers` in the root of the CDK project with the above sample code in `cfnresponse.py` and the handler code in `ddb-stream.py`:

```python
from cfnresponse import send, SUCCESS

import boto3
import botocore
import json

ddb_client = boto3.client('dynamodb')

def handler(event, context):
    print("Received event: " + json.dumps(event, indent=2))

    props = event['ResourceProperties']
    table_name = props['TableName']

    if event['RequestType'] != 'Delete':

        try:
            ddb_client.update_table(
                TableName=table_name,
                StreamSpecification={
                    'StreamEnabled': True,
                    'StreamViewType': 'NEW_IMAGE',
                },
            )
        except botocore.exceptions.ClientError as err:
            if err.response['Error']['Code'] == 'ValidationException':
                print("Table already have stream enabled, skip.")
            else:
                raise err


    resp = ddb_client.describe_table(
        TableName=table_name,
    )
    stream_arn = resp['Table']['LatestStreamArn']

    send(event, context, SUCCESS, {'LatestStreamArn': stream_arn}, physicalResourceId=table_name)

```

This function expects a property `TableName` supplied in the CDK stack. Then the following CDK code creates the lambda function by referring to this code:

```typescript
const ddbStreamFn = new lambda.Function(this, `DdbStreamFn`, {
  runtime: lambda.Runtime.PYTHON_3_7,
  code: lambda.Code.fromAsset('./custom-resource-handlers/'),
  handler: 'ddb-stream.handler',
});

if(ddbStreamFn.role) {
  ddbStreamFn.role.addToPrincipalPolicy(new iam.PolicyStatement({
    effect: Effect.ALLOW,
    actions: ['dynamodb:updateTable', 'dynamodb:describeTable'],
    resources: ['*'],
  }));
}
```

Then supply the Lambda function to a CustomResource and get the output:

```typescript
const tableName = 'TestTable';
const customResource = new CustomResource(this, `${tableName}CustomResource`, {
  serviceToken: ddbStreamFn.functionArn,
  properties: {
    TableName: tableName,
  },
});
const streamArn = customResource.getAtt('LatestStreamArn').toString();
```

## Via Provider Framework
CDK's [Provider framework](https://docs.aws.amazon.com/cdk/api/latest/docs/core-readme.html#custom-resource-providers) removes some sharp edges around CloudFormation's integration with Lambda, particularly calling the pre-signed URL with custom logic. Provider uses the return value of the Lambda function to extract and supply the requested data to CloudFormation. Instead of supplying the Lambda function's ARN to Custom Resource, you create a Provider and supply the function to the `onEventHandler`:

```typescript
// Lambda function that executes the custom logic
const ddbStreamFn = new lambda.Function(this, `DdbStreamFn`, {
  runtime: lambda.Runtime.PYTHON_3_7,
  code: lambda.Code.fromAsset('./custom-resource-handlers/'),
  handler: 'ddb-stream.handler',
});
if(ddbStreamFn.role) {
  ddbStreamFn.role.addToPrincipalPolicy(new iam.PolicyStatement({
    effect: Effect.ALLOW,
    actions: ['dynamodb:updateTable', 'dynamodb:describeTable'],
    resources: ['*'],
  }));
}

// Provider that invokes the lambda function
const ddbStreamProvider = new customresources.Provider(this, 'DdbStreamCustomResourceProvider', {
  onEventHandler: ddbStreamFn,
});

// The custom resource that uses the provider to supply value
const tableName = 'TestTable';
const customResource = new CustomResource(this, `${tableName}CustomResource`, {
  serviceToken: ddbStreamProvider.serviceToken,
  properties: {
    TableName: tableName,
  },
});

// The result obtained from the output of custom resource
const streamArn = customResource.getAtt('LatestStreamArn').toString();
```

The Provider framework can support asynchronous processes by supplying the optional `isCompleteHandler`. This allows the Custom Resource to stabilize beyond the 15-minute maximum execution time by Lambda. Here we only use the synchronous process.

The Lambda function can be simplified as it no longer requires calling the CloudFormation-provided pre-signed URL with the outcome. The result should simply be returned by the Lambda handler. The Lambda function is invoked with the same input from CloudFormation discussed in the previous section.

```python
import boto3
import botocore
import json

ddb_client = boto3.client('dynamodb')

def handler(event, context):
    print("Received event: " + json.dumps(event, indent=2))

    props = event['ResourceProperties']
    table_name = props['TableName']

    if event['RequestType'] != 'Delete':

        try:
            ddb_client.update_table(
                TableName=table_name,
                StreamSpecification={
                    'StreamEnabled': True,
                    'StreamViewType': 'NEW_IMAGE',
                },
            )
        except botocore.exceptions.ClientError as err:
            if err.response['Error']['Code'] == 'ValidationException':
                print("Table already have stream enabled, skip.")
            else:
                raise err


    resp = ddb_client.describe_table(
        TableName=table_name,
    )
    stream_arn = resp['Table']['LatestStreamArn']
    output = {
        'PhysicalResourceId': table_name,
        'Data': {
            'LatestStreamArn': stream_arn
        }
    }
    print("Output: " + json.dumps(output))
    return output
```

The return value of the Lambda function must be a map with the following string-typed keys:

* `"PhysicalResourceId"`
* `"Data"`: a map that contains result available to the CDK stack via `getAtt` method on the `CustomResource` object.

Any other values in the map will be passed through to `isCompleteHandler`.

## On `PhysicalResourceId`
Every resource in CloudFormation has a physical resource ID. When a resource is created, the `PhysicalResourceId` returned from the `Create` operation is stored by CloudFormation and assigned to the logical ID defined for this resource in the template. If a `Create` operation returns without a `PhysicalResourceId`, the provider framework will use RequestId as the default. The sample code for CloudFormation integration uses the log stream name.

When an `Update` operation occurs, the default behavior is to return the current physical resource ID. if the `onEvent` returns a `PhysicalResourceId` which is different from the current one, AWS CloudFormation will treat this as a **resource replacement**, and it will issue a subsequent `Delete` operation for the old resource. Therefore, your Lambda function will be invoked twice with the second one being `Delete`. This may not be what you would expect.

Therefore, as a rule of thumb, if your custom resource relates to a resource, you must return the unique identifier in `PhysicalResourceId` and make sure to handle replacement properly. In my example, the Lambda function always return the name of the DynamoDB table as `PhysicalResourceId` and do not disable the DynamoDB stream even on `Delete`.

## On Singleton Lambda Function
If the Custom Resource is created multiple times to perform the same operation on multiple resources. When defining resources for a custom resource provider, you will likely want to define them as a stack _singleton_ so that only a single instance of the provider is created in your stack and which is used by all custom resources of that type. A typical technique is the have a unique ID for the custom resource provider that can be looked up by `stack.node.tryFindChild(uniqueId)`. For example, the following function creates or returns the previously-defined provider:

```typescript
getOrCreateDdbStreamCustomProvider(scope: Construct): customresources.Provider {
  const stack = Stack.of(scope);
  const uniqueId = 'DdbStreamCustomResourceProvider';

  const existing = stack.node.tryFindChild(uniqueId);
  if(existing === undefined) {
    const ddbStreamFn = new lambda.Function(this, `DdbStreamFn`, {
      runtime: lambda.Runtime.PYTHON_3_7,
      code: lambda.Code.fromAsset('./custom-resource-handlers/'),
      handler: 'ddb-stream.handler',
    });

    if(ddbStreamFn.role) {
      ddbStreamFn.role.addToPrincipalPolicy(new iam.PolicyStatement({
        effect: Effect.ALLOW,
        actions: ['dynamodb:updateTable', 'dynamodb:describeTable'],
        resources: ['*'],
      }));
    }

    const ddbStreamProvider = new customresources.Provider(this, uniqueId, {
      onEventHandler: ddbStreamFn,
    });

    return ddbStreamProvider;
  } else {
    return existing as customresources.Provider;
  }
}
```

Note, CDK uses a tree structure and `stack.node` is the node in which Custom Resources are instantiated. `tryFindChild` only looks at the direct children of the `node`; it does not recursively traverse the tree. In other words, if the same custom resource `A` is created under Construct `X` and `Y`, i.e., the resource is `X/A` and `Y/A` respectively, they will be treated as different resources instead of a singleton.
