---
weight: 5
title: CloudTrail
description: CloudTrail Immersion Day Lab
---

#### Introduction

AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail. Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs. 

AWS CloudTrail will only show the results of the CloudTrail Event History for the current region you are viewing for the last 90 days and support the AWS services found [here](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events-supported-services.html). These events are limited to management events with create, modify, and delete API calls and account activity. For a complete record of account activity, including all management events, data events, and read-only activity, you’ll need to configure a CloudTrail trail.

In this lab, we will create a trail and configure it to send events to CloudWatch Logs.  Then use CloudWatch Logs to monitor your account for specific API calls and events. 
    
#### Labs

[Setup](setup)

[Alerts](alerts)

[Custom Metrics](metrics)

[Log Insights](insights)