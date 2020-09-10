---
weight: 1
title: AWS System Manager
description: This workshop is intended to provide a 200-300 level hands on experience with core AWS Systems Manager capabilities.
---

# AWS Systems Manager Workshop Introduction

This workshop is intended to provide a 200 level hands on experience with core AWS Systems Manager capabilities. Lets start with a brief overview of AWS Systems Manager capabilities: 

1.	**Resource Groups** - Group AWS resources together by any purpose or activity you choose, such as application, environment, region, project, campaign, business unit, or software lifecycle.
2.	**Documents** - Centrally define the configuration options, policies, and the actions that Systems Manager performs on your managed instances and other AWS resources.
3.	**Run Command** - Run a command, with rate and error controls, that targets an entire fleet of managed instances.
4.	**Session Manager** - Securely connect to a managed instance with a single click, without having to open an inbound port or manage SSH keys. 
5.	**Distributor** - Distributor lets you package your own software - or find AWS-provided software packages - to install on AWS Systems Manager managed instances.
6.	**State Manager** - Use and create runbook-style SSM documents that define the configuration state for your managed instances or other AWS resources.
7.	**Patch Manager** – Simplify your operating system patching process for Windows and Linux.
8.	**Maintenance Window** - Automatically perform tasks during defined windows of time.
9.	**Compliance** - Quickly see which resources in your account are out of compliance and take corrective action from a centralized dashboard.
10.	**Parameter Store** - Separate your secrets and configuration data from your code by using parameters, with or without encryption, and then reference those parameters from a number of other AWS services.
11.	**Inventory** - Perform automated inventory by collecting metadata about your Amazon EC2, on-premises, or other cloud managed instances. Metadata can include information about applications, network configurations, and more.
12.	**Automation** - Automate or schedule a variety of operation, maintenance, and deployment tasks.
13.	**OpsCenter** - Centrally view, investigate, and resolve operational work items related to AWS resources.
14.	**Change Calendar** - Create calendar events to allow or block changes to your AWS resources.
15.	**Explorer** - View consolidated operations data from multiple AWS accounts and Regions that you manage. 
16.	**AppConfig** - Create, manage, and safely deploy application configuration data to your targets at runtime.
17.	**Hybrid Activations** - Create an activation to register on-premises servers and virtual machines (VMs), non-AWS Cloud servers, and other devices with AWS Systems Manager. Centrally manage Amazon EC2 instances and your hybrid environment from one location.

## Hands-on Labs Agenda
- [Hands-on Labs Setup](./setup.md)
- **AWS Systems Manager Foundational Knowledge**
  - [Resource Groups](./resourcegroups_tags.md) 
  - [Documents](./documents.md)
  - [Hybrid Activations](./hybridactivations.md)
- **AWS Systems Manager Run Command and Automation**
  - [Run Command](./runcommand.md)
  - [Automation](./automation.md)
- **AWS Systems Manager State Manager** 
  - [State Manager](./statemanager.md)
- **AWS Systems Manager Distributor**
  - [Distributor](./distributor.md)
- **AWS Systems Manager Session Manager** 
  - [Session Manager](./sessionmanager.md)
  - [Port Forwarding with Session Manager](./sessionmanager-portforward.md)
- **AWS Systems Manager Patch Manager**
  - [Patch Manager](./patchmanager.md)
- **AWS Systems Manager Maintenance Window and Change Calendar** 
  - [Maintenance Window](./maintenancewindow.md)
  - [Change Calendar](./changecalendar.md)
- **AWS Systems Manager Compliance, OpsCenter, and Explorer** 
  - [Compliance](./compliance.md)
  - [OpsCenter](./opscenter.md)
  - [Explorer](./explorer.md)
- **AWS Systems Manager Inventory**
  - [Inventory](./inventory.md)
- **AWS Systems Manager Parameter Store** 
  - [Parameter Store](./parameterstore.md)
- **AWS AppConfig**
  - [AppConfig](./appconfig.md)

## Accessing AWS Account for Hands-on Labs

1.	Browse to https://dashboard.eventengine.run 
2.	Log in with the team hash that is provided to you – This will be your own account
![](./media/image1.png)

3.	Log into the console using the provided Login Link

![](./media/image2.png)
	 
**NOTE:** Once the event is complete, the account will be deleted.  

Please proceed to the Setup lab. 