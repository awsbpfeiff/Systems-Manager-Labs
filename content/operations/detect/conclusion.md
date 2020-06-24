---
weight: 5
title: "Conclusion"
---

##### Controls (Management and Governance)

- You have configured automated security controls using [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- You have implemented governance for checking configuration drift using [AWS CloudFormation Drift Detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html) 

##### Observability

- [AWS X-Ray](https://aws.amazon.com/xray/) was preconfigured for you during the setup
- You installed and configured [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) and [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the previous section of the lab
- You have used [CloudWatch Synthetics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html) to **detect** issues in your application workflow

##### Anticipating failure

- You have used [Amazon CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) to help you **detect** or predict possible issues

##### Events

- You created a [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) to warn about anomalous behaviour
- You created a [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) to warn about workflow errors
- You will respond to [CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the next section of the lab