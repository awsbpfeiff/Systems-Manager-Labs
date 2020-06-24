---
weight: 8
title: "Clean Up"
---
 
#### Lab tear down
 
To remove all the resources configured and deployed as part of this lab perform the following: 
 
1. Navigate to the CloudFormation console at this link 
    - Select your stack, choose Actions, and choose Delete stack 
    - This will delete all resources created by the CloudFormation template 
2. Navigate to the Logs page on the CloudWatch console at this link 
    - Select /aws/lambda/ExampleCorpRebootedEvent, then choose Actions, then choose Delete log group, and finally choose Yes, Delete to delete the Log Group 
    - Select application.log, then choose Actions, then choose Delete log group, and finally choose Yes, Delete to delete the Log Group 
    - Select boot.log, then choose Actions, then choose Delete log group, and finally choose Yes, Delete to delete the Log Group 
    - Select messages, then choose Actions, then choose Delete log group, and finally choose Yes, Delete to delete the Log Group 
    - Select production.log, then choose Actions, then choose Delete log group, and finally choose Yes, Delete to delete the Log Group 
3. Navigate to the Metrics page on the CloudWatch console at this link 
    - CloudWatch does not support metric deletion. Metrics expire based on the retention schedules which can be found at this link 
4. Navigate to the Alarms page on the CloudWatch console at this link 
    - Select ExampleCorpRebootedAlarm by checking the box on the left in its row. 
    - Choose Actions, choose Delete, and finally choose Yes, Delete to delete the Alarm 
5. Navigate to the Dashboards page of the CloudWatch console at this link 
    - Choose ExampleCorp 
    - Choose Actions, choose Delete dashboard, and finally when prompted select Delete dashboard to confirm that you wish to delete your dashboard. 
 
#### References: ExampleCorp 
 
A mock workload for a fictitious company. 
 
- Source code: https://github.com/horsfieldsa/example-corp-app 
- Admin interface: http://<WebAppAppUrl>/admin 
- Admin user: admin@admin.com 
- Admin password: Password123 
 
**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 