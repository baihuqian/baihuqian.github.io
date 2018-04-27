---
layout: post
title: 'AWS Well-Architected Framework: A Note'
tags: AWS
---

This is the note I made when studying [AWS Well-Architected Framework](http://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf).

The AWS Well-Architected Framework is based on five pillars — operational excellence, security, reliability, performance efficiency, and cost optimization. When architecting solutions you make trade-offs between pillars based upon your business context. These business decisions can drive your engineering priorities.

{:toc}

## General Design Principles
The Well-Architected Framework identifies a set of general design principles to
facilitate good design in the cloud:

- Stop guessing your capacity needs. Cloud computing enables you to scale up and down automatically based on your needs.
- Test systems at production scale. In the cloud, you can create a production-scale test environment on demand, complete your testing, and then decommission the resources.
- Automate to make architectural experimentation easier. Automation allows you to create and replicate your systems at low cost, avoid the expense of manual effort, and put your changes into version control.
- Allow for evolutionary architectures.  In the cloud, the capability to automate and test on demand lowers the risk of impact from design changes. This allows systems to evolve over time so that businesses can take advantage of innovations as a standard practice.
- Drive architectures using data. You can collect data for your architectures in the cloud and make fact-driven changes to your workloads.
- Improve through game days. Test how your architecture and processes perform by regularly scheduling game days to simulate events in production. This will help you understand where improvements can be made and can help develop organizational experience in dealing with events.

## The Five Pillars of the Well-Architected Framework
### Operational Excellence

The operational excellence pillar includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures. [More details can be found in this whitepaper.](https://d0.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf)

There are six design principles for operational excellence in the cloud:

- Perform operations as code to limit human error and enable consistent responses to events. Your entire workload in the cloud (applications, infrastructure, etc.) can be defined as code and updated it with code. You can script your operations procedures and automate their execution by triggering them in response to events.
- Annotate documentation. In the cloud, you can automate the creation of documentation after every build (or automatically annotate hand-crafted documentation). Annotated documentation can be used by
people and systems. Use annotations as an input to your operations code. This standardizes the production of your documentation and eliminates the possibility of out-of-sync between the documentation and your system.
- Make frequent, small, reversible changes. Design workloads to allow components to be updated regularly. Make changes in small increments that can be reversed if they fail (without affecting customers when possible). Larger changes are almost always more dangerous to update, prone to error, and harder to be reversed.
- Refine operations procedures frequently. As your workload evolves, operations procedures get out-of-date quickly. Stale operations procedures jeopardize operational excellence of your system and prolong mitigation, isolation, and recovery in events. Set up regular game days to review and validate that all procedures are effective and that teams are familiar with them.
- Anticipate failure. Perform “pre-mortem” exercises to identify potential sources of failure so that they can be removed or mitigated.
- Learn from all operational failures: Drive improvement through lessons learned from post-mortem analyses of all operational events and failures. Share what is learned across teams and through the entire organization.

Operations teams need to understand their business and customer needs so they can effectively and efficiently support business outcomes. Operations creates and uses procedures to respond to operational events and validates their effectiveness to support business needs. Operations collects metrics that are used to measure the achievement of desired business outcomes. Everything continues to change—your business context, business priorities, customer needs, etc. It’s important to design operations to support evolution over time in response to change and to incorporate lessons learned through their performance.

There are three areas of best practices: Prepare, Operate, and Evolve.

AWS CloudFormation can be used to create templates based on best practices.

#### Prepare
- Create mechanisms to validate that workloads, or changes, are ready to be moved into production and supported by operations.
- Operational readiness is validated through checklists to ensure a workload meets defined standards and that required procedures are adequately captured in runbooks and playbooks.
- Invest in scripting operations activities to maximize the productivity of operations personnel, minimize error rates, and enable automated responses.
- Adopt deployment practices that take advantage of the elasticity of the cloud to facilitate pre-deployment of systems for faster implementations.

AWS Config and AWS Config rules can be used to create standards for workloads and to determine if environments are compliant with those standards before being put into production.

#### Operate
Successful operation of a workload is measured by the achievement of business and customer outcomes. Define expected outcomes, determine how success will be measured, and identify the workload and operations metrics that will be used in those calculations to determine if operations are successful.

- Successful operation of a workload is measured by the achievement of business and customer outcomes.
- Define expected outcomes, determine how success will be measured, and identify the workload and operations metrics that will be used in those calculations to determine if operations are successful.
- Communicate the operational status of workloads through dashboards and notifications that are tailored to the target audience (for example, customer, business, developers, operations) so that they may take appropriate action, so that their expectations are managed, and so that they are informed when normal operations resume.
- Determine the root cause of unplanned events and unexpected impacts from planned events.
- Routine operations should be automated, and manual processes for deployments, release management, changes, and rollbacks should be avoided.

Amazon CloudWatch allows you to monitor the operational health of a workload.

#### Evolve
- Regularly evaluate and prioritize opportunities for improvement (for example, feature requests, issue remediation, and compliance requirements), including both the workload and operations procedures.
- Share lessons learned across teams to share the benefits of those lessons.
- Analyze trends within lessons learned and perform cross-team retrospective analysis of operations metrics to identify opportunities and methods for improvement.

Successful evolution of operations is founded in: frequent small improvements; providing safe environments and time to experiment, develop, and test improvements; and environments in which learning from failures is encouraged.

Amazon Elasticsearch Service (Amazon ES) allows you to analyze your log data to gain actionable insights quickly and securely.

### Security
The security pillar includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.

There are six design principles for security in the cloud:
* Implement a strong identity foundation. Implement the principle of least privilege and enforce separation of duties with appropriate authorization for each interaction with your AWS resources. Centralize privilege management and reduce or even eliminate reliance on longterm credentials.
* Enable traceability: Monitor, alert, and audit actions and changes to your environment in real time. Integrate logs and metrics with systems to automatically respond and take action.
* Apply security at all layers.
* Automate security best practices.
* Protect data in transit and at rest. Classify your data. Reduce or eliminate direct human access to data.
* Prepare for security events.

There are five best practice areas.

#### Identity and Access Management
Identity and access management ensure that only authorized and authenticated users are able to access your resources, and only in a manner that is intended. Use **AWS Identity and Access Management (IAM)** service to achieve privilege management.

#### Detective Controls
You can use detective controls to identify a potential security incident. In AWS, you can implement detective controls by processing logs, events, and monitoring that allows for auditing, automated analysis, and alarming. **CloudTrail** logs, AWS API calls, and **CloudWatch** provide monitoring of metrics with alarming, and AWS Config provides configuration history. Service-level logs are also available, for example, you can use Amazon Simple Storage Service (Amazon S3) to log access requests. Finally, Amazon Glacier provides a vault lock feature to preserve mission-critical data with compliance controls designed to support auditable long-term retention.

#### Infrastructure Protection
In AWS, you can implement stateful and stateless packet inspection. You should use **Amazon Virtual Private Cloud (Amazon VPC)** to create a private, secured, and scalable environment in which you can define your topology. Multiple layers of defense are advisable in any type of environment. In the case of infrastructure protection, many of the concepts and methods are valid across cloud and on-premises models. Enforcing boundary protection, monitoring points of ingress and egress, and comprehensive logging, monitoring, and alerting are all essential to an effective information security plan.

#### Data Protection
Data classification provides a way to categorize organizational data based on levels of sensitivity, and encryption protects data by rendering it unintelligible to unauthorized access. AWS provides multiple means for encrypting data at rest and in transit. We build features into our services that make it easier to encrypt your data. For example, we have implemented server-side encryption (SSE) for Amazon S3 to make it easier for you to store your data in an encrypted form. You can also arrange for the entire HTTPS encryption and decryption process (generally known as SSL termination) to be handled by Elastic Load Balancing (ELB).

Services such as ELB, Amazon Elastic Block Store (Amazon EBS), Amazon S3, and Amazon Relational Database Service (Amazon RDS) include encryption capabilities to protect your data in transit and at rest. Amazon Macie automatically discovers, classifies, and protects sensitive data, while AWS Key Management Service (AWS
KMS) makes it easy for you to create and control keys used for encryption.

#### Incident Response
In AWS, the following practices facilitate effective incident response:
* Detailed logging is available that contains important content, such as file access and changes.
* Events can be automatically processed and trigger scripts that automate runbooks through the use of AWS APIs.
* You can pre-provision tooling and a “clean room” using AWS CloudFormation. This allows you to carry out forensics in a safe, isolated environment.

### Reliability
The reliability pillar includes the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.

There are five design principles for reliability in the cloud:
* Test recovery procedures. In the cloud, you can test how your system fails, and you can validate your recovery procedures. You can use automation to simulate different failures or to recreate scenarios that led to failures before.
* Automatically recover from failure.
* Scale horizontally to increase aggregate system availability. Replace one large resource with multiple small resources to reduce the impact of a single failure on the overall system.
* Stop guessing capacity.
* Manage change in automation.

To achieve reliability, a system must have a well-planned foundation and monitoring in place, with mechanisms for handling changes in demand or requirements. The system should be designed to detect failure and automatically heal itself.There are three best practice areas.

#### Foundations
Foundational requirements, such as service limits, network bandwidth requirements and topology.You will need to have governance and processes in place to monitor and change AWS service limits to meet your business needs. In a hybrid model, it’s important to have a design for how your AWS and onpremises resources will interact as a network topology.

#### Change Management
Being aware of how change affects a system allows you to plan proactively, and monitoring allows you to quickly identify trends that could lead to capacity issues or SLA breaches. When you architect a system to automatically add and remove resources in response to changes in demand, this not only increases reliability but also ensures that business success doesn’t become a burden. With monitoring in place, your team will be automatically alerted when KPIs deviate from expected norms. Automatic logging of changes to your environment allows you to audit and quickly identify actions that might have impacted reliability. Controls on change management ensure that you can enforce the rules that deliver the reliability you need.

#### Failure Management
In any system of reasonable complexity it is expected that failures will occur. It is generally of interest to know how to become aware of these failures, respond to them, and prevent them from happening again.

With AWS, you can take advantage of automation to react to monitoring data. Rather than trying to diagnose and fix a failed resource that is part of your production environment, you can replace it with a new one and carry out the analysis on the failed resource out of band. Since the cloud enables you to stand up temporary versions of a whole system at low cost, you can use automated testing to verify full recovery processes. A key to managing failure is the frequent and automated testing of systems to cause failure, and then observe how they recover.  The objective is to thoroughly test your system-recovery processes so that you are confident that you can recover all your data and continue to serve your customers, even in the face of sustained problems.

### Performance Efficiency
The performance efficiency pillar includes the ability to use computing resources efficiently to meet system requirements and to maintain that efficiency as demand changes and technologies evolve.

There are five design principles for performance efficiency in the cloud:

* Democratize advanced technologies by using managed services.
* Go global.
* Use serverless architectures.
* Experiment more often.
* Mechanical sympathy. Use the technology approach that aligns best to what you are trying to achieve.

#### Selection
Well-architected systems use multiple solutions and enable different features to improve performance. In AWS, resources are virtualized and are available in a number of different types and configurations. This makes it easier to find an approach that closely matches your needs, and you can also find options that are not easily achievable with on-premises infrastructure. Use a data-driven approach for the most optimal solution. data obtained through benchmarking or load testing will be required to optimize your architecture.

* Compute: The optimal compute solution for a particular system may vary based on application design, usage patterns, and configuration settings. In AWS, compute is available in three forms: instances, containers, and functions.
* Storage: The optimal storage solution for a particular system will vary based on the kind of access method (block, file, or object), patterns of access (random or sequential), throughput required, frequency of access (online, offline, archival), frequency of update (WORM, dynamic), and availability and durability constraints.
* Database: The optimal database solution for a particular system can vary based on requirements for availability, consistency, partition tolerance, latency,
durability, scalability, and query capability. Many systems use different database solutions for various subsystems and enable different features to improve performance.
* Network: The optimal network solution for a particular system will vary based on latency, throughput requirements and so on. Physical constraints such as user or onpremises resources will drive location options, which can be offset using edge techniques or resource placement.

#### Review
Over time new technologies and approaches become available that could improve the performance of your architecture. Understanding where your architecture is performance-constrained will allow you to look out for releases that could alleviate that constraint.

#### Monitoring
you will need to monitor its performance so that you can remediate any issues before your customers are aware. Monitoring metrics should be used to raise alarms when thresholds are breached. The alarm can trigger automated action to work around any badly performing components. Ensuring that you do not see too many false positives, or are overwhelmed with data, is key to having an effective monitoring solution.

#### Tradeoffs
Depending on your situation you could trade consistency, durability, and space versus time or latency to deliver higher performance. Tradeoffs can increase the complexity of your architecture and require load testing to ensure that a measurable benefit is obtained.

### Cost Optimization
* Adopt a consumption model: Pay only for the computing resources that you consume and increase or decrease usage depending on business requirements, not by using elaborate forecasting.
* Measure overall efficiency: Measure the business output of the system and the costs associated with delivering it. Use this measure to understand the gains you make from increasing output and reducing costs.
* Stop spending money on data center operations.
* Analyze and attribute expenditure.
* Use managed services to reduce cost of ownership.

#### Cost-Effective Resources
A well-architected system will use the most cost-effective resources, which can have a significant and positive economic impact. By using tools such as AWS Trusted Advisor to regularly review your AWS usage, you can actively monitor your utilization and adjust your deployments accordingly.

#### Matching Supply and Demand
Optimally matching supply to demand delivers the lowest cost for a system, but there also needs to be sufficient extra supply to allow for provisioning time and individual resource failures. Demand can be fixed or variable, requiring metrics and automation to ensure that management does not become a significant cost.

#### Expenditure Awareness
You can use cost allocation tags to categorize and track your AWS costs. When you apply tags to your AWS resources (such as EC2 instances or S3 buckets), AWS generates a cost allocation report with your usage and costs aggregated by your tags. You can apply tags that represent business categories (such as cost centers, system names, or owners) to organize your costs across multiple services.

#### Optimizing Over Time
As AWS releases new services and features, it is a best practice to review your existing architectural decisions to ensure they continue to be the most cost-effective. As your requirements change, be aggressive in decommissioning resources, entire services, and systems that you no longer require. Managed services from AWS can often significantly optimize a solution, so it is good to be aware of new managed services as they become available.
