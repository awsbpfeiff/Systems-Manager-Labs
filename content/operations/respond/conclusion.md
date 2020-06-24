---
weight: 5
title: "Conclusion"
---

##### Identify trends

- You already used [Amazon CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) in a previous section. 
- In this section, you used [Amazon CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) to troubleshoot the root cause of the issue, enabling you to create a metric, alert and automation.

##### Ensure Consistency

- In a previous section you ensure consistent patching levels using [AWS Systems Manager](https://aws.amazon.com/systems-manager/)
- You used [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) to deploy your application

##### Validate responses

- You confirmed that your website auto-recovered after you create a [CloudWatch Metric Filter](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) to trigger a [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

##### Automate responses

- You configured a [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) to automatically respond to an issue using [AWS Lambda](https://aws.amazon.com/lambda/) 