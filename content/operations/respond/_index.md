---
weight: 4
title: "Respond"
---

#### Introduction

##### Identify trends

To avoid impact, use detection processes to identify trends that point to potential availability impacting events. This can be as simple as dashboards and manual correlation, but can mature into machine learning algorithms and AI Ops.

- [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- [AWS CloudTrail Insights](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-insights-events-with-cloudtrail.html)
- [Amazon CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)
- [Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)
- [Amazon CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html)

##### Ensure Consistency

Build, operate and maintain consistency. Do not be left guessing what to do when events have Ops teams under the duress of an impending or ongoing outage. Have runbooks for known types of unplanned Operational events, and playbooks for events that might not have a well-understood cause. Make sure all response activities are scripted and validated actions, manually typing commands increases the potential for human error. 

- [AWS Systems Manager](https://aws.amazon.com/systems-manager/)
- [AWS OpsWorks](https://aws.amazon.com/opsworks/)
- [AWS Config](https://aws.amazon.com/config/)

##### Validate responses

Have you confirmed that your responses deliver the desired outcome? Ensure that you test your processes, test for failure and the technical implementation of backup recovery and disaster recovery. If you have a disaster recovery strategy, don't forget to test your failback process as well.

- [GameDays](https://wa.aws.amazon.com/wat.concept.gameday.en.html)
- [Chaos engineering](https://github.com/Netflix/chaosmonkey)
- [Disaster recovery testing](https://aws.amazon.com/blogs/database/implementing-a-disaster-recovery-strategy-with-amazon-rds/)

##### Automate responses

If a response can be automated to avoid or minimise impact, it should be. Automation can be used to avoid business impact by scaling in response to increased traffic or to minimize impact by automating a runbook in response to an incident.

Automation should be based on thorough testing with known and expected results. Bad automation can lead to bad outcomes!

- [AWS Auto Scaling](https://aws.amazon.com/autoscaling/) 
- [AWS Lambda](https://aws.amazon.com/lambda/)
- [AWS Shield](https://aws.amazon.com/shield/)
- [AWS Systems Manager](https://aws.amazon.com/systems-manager/)