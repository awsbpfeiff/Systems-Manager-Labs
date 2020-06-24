---
weight: 1
title: "Setup"
---
 
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
  - Data Events (Provide Insights into the resource operations) 
    - Check the Box on Select all S3 Buckets in your account 
    - Click on the Lambda Tab, and check the Box Log all Current and Future Functions 
  - Create a new S3 Bucket: *Yes* 
    - S3 bucket: *management-tools-week-\(**Today'sDate**\)-\(**yourcellnumber**\)-* 
      - We are using Cell Phone Number at the End to ensure that we create a Uniquie Bucket per user. For more information on Bucket Restrictions and Limitations click [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) 
  - Click on Create 
5. Lets setup our trail to send logs to CloudWatch so we can search through them a bit easier. 
  - Click back into the Trail, Go to CloudWatch Logs Section and click on Configure 
 
      ![Add CloudWatch Log to Trail](../images/ConfigCWLogCloudtrail.jpg) 
 
  - In the New or exisiting log group, Type the Following name *ManagementToolsWeek/CloudTrail* and then click continue 
  - Next Screen Click Allow, this Gives Cloudtrail the ability to assume a role to write to CloudWatch Logs. 
 
We now have a trail capturing activity in our AWS Account. Later on, we will search through our trail. 
 
#### Turn on AWS Config
 
AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time. 
 
1. Search for the Config Service under the Management Tools Section in the console and click on Config. 
 
    ![Get to AWS Config Console](../images/ConfigConsole.jpg) 
 
2. Click on Getting Started, lets follow the Setup Wizard 
  - Keep all Defaults and Click Next – This will Create an S3 Bucket, a Role for the Config Service, and will record all resources supported by Config within the region. For a list of supported Service click [here](https://docs.aws.amazon.com/config/latest/developerguide/resource-config-reference.html). 
  - Click Next on the Next Screen, we will setup Config Rules later in the day. 
  - On the Last Screen Click on Confirm. 
 
We now have AWS Config recording changes for supported resources. 