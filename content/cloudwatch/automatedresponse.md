---
weight: 6
title: "Automate responses"
---

#### Build a Monitoring Plan: Alerting and Response
 
Monitoring for Operational Outcomes Sansa Bailish is focused on outages, reliability, and getting better sleep. There is a known issue with image trends; when the instances are rebooted the application doesn't restart. Too frequently Sansa has received a late night call to get online and restart the application. The user experience lead is very frustrated with the downtime associated to these incidents. They need to be detected sooner and resolved faster. 
 
Sansa is looking for a monitoring solution to detect the incident (the reboot event), and a way to trigger an automated recovery. 
 
#### Create and Automate Responses 
 
Create a Runbook to Resolve the Error 
 
**6.1 Resolve the error manually** 
 
1. Depending on your browser, you will see an error showing that the page cannot be loaded. 
2. Add /admin to the end of the URL from the CloudFormation output. 
3. Click on images in the menu to see the images with the error. 
4. Delete the image by pressing the x next to the image with no tags or errors in the tags. 
5. Click Home on the navigation bar at the top 
**NOTE:** The application should be working again.  
 
**6.2 Automate Responses using Lambda** 
 
1. Navigate to the Lambda Console 
2. Click on the ExampleCorpErrorEvent Lambda function 
3. Under Basic settings 
    - Set Timeout to 30 seconds 
    - Under Network 
        - Select the VPC created earlier with a 10.180.0.0/16 CIDR block for VPC 
        - Select ...App Subnet (AZ1) under Subnets 
        - Select ...App Subnet (AZ2) under Subnets 
        - Select ...AppHostSecurityGroup... under Security groups 
4. Click on the Save button 
 
**6.3 Validate - Ensure the response has the desired outcome** 
 
1. Log in to ExampleCorp using the URL that you made a note of earlier (CloudFormation Output) in a new browser tab. 
2. Enter admin@admin.com for email 
3. Enter Password123for Password 
4. Click Login 
5. Click Upload Image 
6. Click Select Image 
7. Find break_app.jpg in your filesystem 
8. Click Upload 
9. It will break again, but it should recover in a few seconds 
10. Keep an eye on the Lambda monitoring page 
 
**6.4: Bonus Content: Review the outcomes in CloudWatch** 
 
1. Click on View logs in CloudWatch 
2. Expand parts of the logs to see what happened. 
3. The Lambda got invoked 
4. You can see the SNS Message 
5. Then you see that 2 rows were deleted 
6. Finally the Lambda execution finishes with some summary statistics. 
7. Click on Alarms 
8. Select ImageErrorAlarm 
9. View the history see the Alarm State update and the Action to notify SNS which then calls the Lambda function. 
 
What have we achieved? 
- We have answered three monitoring needs, one for each of our teams. 
- We have provided insight to the quality of the end user experience by creating a image tag confidence metric for the business team. 
- We have provided insight to issues and potential development needs by providing the development team (DevOps) with error and warning metrics. 
- We have provided a mechanism for the operations team (SRE) to use an event trigger to automatically remediate an outage causing issue in the environment. 
 
How did we create an automatic remediation? 
- Knowing the log entry that indicates that the application has failed and is in a state from which it can be recovered we created a metric. 
- Using that metric we created an alarm that notifies via an SNS topic. 
- We have a Lambda function that is subscribed to that SNS topic that creates a CloudWatch Event in response. 
- We created a CloudWatch rule that triggers on our Lambda initiated CloudWatch event and invokes run command. 
- Our run command invocation uses a command document we created to execute the start up script on our server restoring it to an operating state. 
 
We have created something of a Rube Goldberg machine to achieve that outcome. In doing so we have demonstrated the use of logs and how to get more value out of them, the use of events, and the triggering of actions in response to events; all enabled by CloudWatch. 
