---
weight: 4
title: "Logs Insights"
---

#### Querying CloudTrail Logs in Logs Insights

CloudWatch Logs Insights enables you to interactively search and analyze your log data in Amazon CloudWatch Logs. You can perform queries to help you more efficiently and effectively respond to operational issues. If an issue occurs, you can use CloudWatch Logs Insights to identify potential causes and validate deployed fixes.

CloudWatch Logs Insights automatically discovers fields in logs from AWS services such as Amazon Route 53, AWS Lambda, AWS CloudTrail, and Amazon VPC, and any application or custom log that emits log events as JSON.  In this lab exercise, we will query CloudTrail events CloudWatch Logs data with Insights.  

1. Go to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home#logs:)
2. From the drop down, select the CloudWatch Log Group created during the setup.
3. In the query pane, enter the following query, which filters failed SignIn attempts to the AWS Account and also captures if MFA was used or not.
    ```
    filter eventSource="signin.amazonaws.com" and eventName="ConsoleLogin" and responseElements.ConsoleLogin="Failure"
    | stats count(*) as Total_Count by sourceIPAddress as Source_IP, errorMessage as Reason, awsRegion as AWS_Region, userIdentity.arn as IAM_Arn, additionalEventData.MFAUsed as MFA_Used
    ```
4. Click on Run Query to view results.
    ![Insights Query Returns](../images/QueryResults.png) 
5. Click on Action and then click on Add to dashboard.
6. In the pop-up window:
 - Click on Create new
 - Give a dashboard name MyFirstDashboard. 
 - Click on the small tick icon to create the dashboard.
7. Click on Add to dashboard.
8. Navigate to MyFirstDashboard under Dashboards from the left navigation pane. View the Widget.
    ![CloudWatch Dashboard](../images/Dashboard.png) 

**Note:** The CloudWatch Logs Insights Console has a few sample queries to start with under Sample queries. Refer this [document](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html) for more information.


**End of Lab Exercises** 
 
**Thank you for using this lab.** 