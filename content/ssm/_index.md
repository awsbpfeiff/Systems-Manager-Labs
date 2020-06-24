---
weight: 1
title: AWS System Manager
description: This workshop is intended to provide a 200-300 level hands on experience with core AWS Systems Manager capabilities.
---

# Workshop Introduction

This workshop is intended to provide a 200-300 level hands on experience with core AWS Systems Manager capabilities. Lets start with a brief overview of AWS Systems Manager capabilities: 

1.	**Resource Groups** - Group AWS resources together by any purpose or activity you choose, such as application, environment, region, project, campaign, business unit, or software lifecycle.
1.	**Documents** - Centrally define the actions that Systems Manager performs, such as patching managed instances, creating backups of Amazon RDS databases, or disabling public read access of Amazon S3 buckets.
1.	**Run Command** - Run a command, with rate controls for concurrency and error thresholds, that targets a subset or an entire fleet of managed instances. 
1.	**Session Manager** - Securely connect to a managed instance with a single click, without having to open an inbound port or managing SSH keys. 
1.	**Distributor** - Distributor lets you package your own software - or find AWS-provided software packages - to install on AWS Systems Manager managed instances.
1.	**State Manager** - Use and create runbook-style SSM documents that define the actions to perform on your managed instances or AWS resources.
1.	**Patch Manager** – Simplify your operating system patching process for Windows and Linux managed instances.
1.	**Maintenance Window** - Automatically perform tasks in defined windows of time.
1.	**Compliance** - Quickly see which resources in your account are out of compliance and take corrective action from a centralized dashboard.
1.	**Parameter Store** - Separate your secrets and configuration data from your code by using parameters, with or without encryption, and then reference those parameters from a number of AWS services.
1.	**Inventory** - Perform automated inventory by collecting metadata about your Amazon EC2 and hybrid (on-premise or other cloud) managed instances. Metadata can include information about applications, network configurations, and more.
1.	**Automation** - Automate or schedule a variety of maintenance and deployment tasks.
1.	**OpsCenter** - Centrally view, investigate, and resolve operational work items related to AWS resources.
1.	**Change Calendar** - Create calendar events to allow or block changes to your AWS resources.
1.	**Explorer** - View consolidated operations data from multiple AWS Regions and accounts that you manage, such as total number of instances, patching compliance status, Trusted Advisor checks, Compute Optimizer recommendations, and more.
1.	**AppConfig** - Create, manage, and safely deploy application configuration data to your targets at runtime.
1.	**Hybrid Activations** - Create an activation to register on-premises servers and virtual machines (VMs), non-AWS Cloud servers, and other devices with AWS Systems Manager. Centrally manage Amazon EC2 instances and your hybrid environment from one location.

### Goals:
* Automated deployment of infrastructure
* Dynamic management of resources
* Automated patch management

### Prerequisites:
* An [AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) that you are able to use for testing, that is not used for production or other purposes.  
* An IAM user or role in your AWS account with full access to CloudFormation, EC2, VPC, IAM.  

>**Important**
If you are not using Event Engine, you will be billed for any applicable AWS resources used in this lab that are not covered in the [AWS Free Tier](https://aws.amazon.com/free/). At the end of the lab guide there is an additional section on how to remove all the resources you have created.


### Permissions required
* IAM User with *AdministratorAccess* AWS managed policy

Lab Sections:
{{% children  %}}