---
weight: 1
title: "Setup"
---

**The US-EAST-1 AWS Region must be used with Event Engine**
 
#### Create a Trail in CloudTrail
 
AWS CloudTrail is an AWS service that helps you enable governance, compliance, risk auditing and operational auditing of your AWS Account. Actions taken by a Principal (User, Role or AWS Service) are recorded as events in CloudTrail. To learn more about AWS CloudTrail you can click on this [link](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html). Documentation on creating a Trail via the Console is located [here](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html#creating-a-trail-in-the-console). We will highlight the steps below. 
 
1. Search for the CloudTrail Service under the Management Tools Section in the console and click on Cloudtrail. 
 
    ![Get to CloudTrail Console](../images/CloudtrailConsole.jpg) 
 
2. Click on getting Started if presented with that screen. Once in the CloudTrail Console, click on Trails on the Left Side of the screen. 
3. Then Click on Create Trail, to create our trail for this lab. 
 
    ![Create Trail](../images/CreateTrail.jpg) 
 
4. Apply the following settings and create the trail 
  - Trail name: *management-tools-week* 
  - Apply trail to all regions: Yes 
  - Read/Write Events: All
  - Log AWS KMS events: No (Encrypt, Decrypt, and GenerateDataKey)
  - Log Insights events: No (May take up to 36 hours to receive the first Insights events)
  - Data Events (Provide Insights into the resource operations) 
    - Check the Box on Select all S3 Buckets in your account 
    - Click on the Lambda Tab, and check the Box Log all Current and Future Functions 
  - Create a new S3 Bucket: *Yes* 
    - S3 bucket: *management-tools-week-\(**Today'sDate**\)-\(**yourcellnumber**\)-* 
      - We are using Cell Phone Number at the End to ensure that we create a Uniquie Bucket per user. For more information on Bucket Restrictions and Limitations click [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) 
  - Click on Create 
 
We now have a trail capturing activity in our AWS Account. Later on, we will search through our trail. 
 
#### Configure CloudWatch Logs to monitor your trail logs
 
You can configure your trail to send events to CloudWatch Logs. You can then use CloudWatch Logs to monitor your account for specific API calls and events. 

1.	Click on the Trail name you just created. (Example: MyFirstTrail)
2.	Scroll down to CloudWatch Logs section. Click Configure.

    ![Trail events to CloudWatch Logs](../images/TrailtoCloudWatchLogs1.png) 

3.	Enter a New or existing log group name. (Example: CloudTrail/MyFirstLogGroup or use the default CloudTrail/DefaultLogGroup)
4.	Click Continue.
5.	You will be navigated to IAM Console page to grant CloudTrail service to assume an IAM Role that you can create or specify under View Details. This step grants IAM Role to two CloudWatch Logs API calls: CreateLogStream and PutLogEvents. For now, let’s leave the default values shown under View Details. Click on Next.
6. If prompted to configure Role and Policy assignment, click Allow to continue.

    ![Trail events to CloudWatch Logs](../images/CloudTrail2CloudWatchRole.png) 

7. When completed, we will see the Log group configuration on the screen.

    ![Trail events to CloudWatch Logs](../images/TrailtoCloudWatchLogs2.png)

#### Create CloudWatch Alarms for Security and Network related API activity

In this section, we will use the pre-defined CloudFormation template to create a set of CloudWatch Alarms to monitor for security and network related activity.

1. Under the CloudWatch Loogs section of Trails Configuration page, click on the  Create CloudWatch Alarms for Security and Network related API activity using CloudFormation template link.  

    ![Create CloudWatch Alarms with CFN](../images/CFNAlarms.png)

2. On the Create stack page, we will click Next.
3. In the Specify stack details page, we will specify a valid e-mail address and the LogGroupName we used in step 5 of the previous section.  Click Next.

    ![CFN specify LogGroupName](../images/CFNAlarmsStackSpecifyLogGroup.png)

4. On the next page, leave the default options and click Next.
5. When you see the Create stack button, click on it.

The CloudFormation template will create various resources, including CloudWatch Alarms and an SNS Topic with a Subscription.  After the CloudFormation template deployment is complete, you will receive an SNS Subscription notifcation.  When you receive, confirm the subscription.

![SNS Subscription confirmation](../images/SubscriptionConfirmation.png)
