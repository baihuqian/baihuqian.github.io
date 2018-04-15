---
layout: post
published: false
title: 'AWS Well-Architected Framework: A Note'
---
This is the note I made when studying [AWS Well-Architected Framework](http://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf).

The AWS Well-Architected Framework is based on five pillars — operational excellence, security, reliability, performance efficiency, and cost optimization. When architecting solutions you make trade-offs between pillars based upon your business context. These business decisions can drive your engineering priorities. 

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
- Refine operations procedures frequently. As your workload evolves, operations procedures get out-of-date quickly. Stale operations procedures jeopordize operational excellence of your system and prolong mitigation, isolation, and recovery in events. Set up regular game days to review and validate that all procedures are effective and that teams are familiar with them.
- Anticipate failure. Perform “pre-mortem” exercises to identify potential sources of failure so that they can be removed or mitigated. 
- Learn from all operational failures: Drive improvement through lessons learned from post-mortem analyses of all operational events and failures. Share what is learned across teams and through the entire organization.

Operations teams need to understand their business and customer needs so they can effectively and efficiently support business outcomes. Operations creates and uses procedures to respond to operational events and validates their effectiveness to support business needs. Operations collects metrics that are used to measure the achievement of desired business outcomes. Everything continues to change—your business context, business priorities, customer needs, etc. It’s important to design operations to support evolution over time in response to change and to incorporate lessons learned through their performance.There are three areas of best practices: Prepare, Operate, and Evovle.

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