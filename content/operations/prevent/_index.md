---
weight: 2
title: "Prevent"
---

#### Introduction

##### Readiness

You should use a consistent process (including checklists) to know when you areready to go live with your workload. This will also enable you to find any areas you will need to make plans to address. You will have runbooks that document your routine activities and playbooks that guide your processes for issue resolution. You will need to have enough team members to cover operational activities (including on-call), with training on AWS, your workload,  and your operations tools.  You should use a governance process to make an informed decision on launching your workload.

 - [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)
 - [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)
 - [AWS Config](https://aws.amazon.com/config/)

##### Awareness

When operating a workload you will need to understand what you have, where they are and what state they are in. Tagging, Configuration Management, Inventory and Patch Managment help you to understand your workload.

 - [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)
 - [AWS Config](https://aws.amazon.com/config/)
 - [AWS Systems Manager](https://aws.amazon.com/systems-manager/)

##### Telemetry

When instrumenting your workload, capture a broad set of information to enable situational awareness (for example, changes in state, user activity, privilege access, utilization counters),  knowing that you can filter to select the most useful information over time. Tag your resources for organization, cost accounting, access controls, and targeting the execution of automation

 - [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
 - [AWS CloudTrail](https://aws.amazon.com/cloudtrail/)
 - [AWS X-Ray](https://aws.amazon.com/xray/)

##### Guardrails

Guardrails are preventive or detective controls that help you govern your resources and monitor compliance across groups of AWS accounts. 

  - [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
  - [Amazon Inspector](https://aws.amazon.com/inspector/)
  - [AWS Service Catalog](https://aws.amazon.com/servicecatalog/)
  - [Guardrails in AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/guardrails.html)