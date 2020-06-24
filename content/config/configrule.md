---
weight: 2
title: "Config Rule"
---

#### Deploy Components for Lab
 
[**Click here To Deploy Lab into your Account**](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=us-east-1&stackName=ConfigSSMLab&templateURL=https://s3.amazonaws.com/workshop.aws-management.tools/config/template/ConfigSSMLab.yml) 
 
**Note :** Make sure to Specify the Same S3 Bucket Name in the Bucket Parameter as is assigned to the CloudTrail Trail.  
 
**AWS Config is a Regional Service, Please make sure to deploy the Lab in the Same Region you enabled AWS Config)** 
 
#### Creating a Config Rule to Alert on SSM Agent
 
In this step we will create a config rule using an AWS Managed rule that will evaluate if Instances have a working AWS Systems Manager Agent. 
 
1. Let's go to the AWS Config Console, once there click on Rules on the left side of the console. 
2. Click on **Add Rule** 
3. In the Add Rule Screen in the Filter section type *ec2-instance-managed-by-systems-manager*, click on the ec2-instance-managed-by-systems-manager rule. 
4. Under the Trigger Section take notice of the Trigger Type, it is a configuration change. Leave these settings as is.  
 ![](../images/ssmagentconfigrule.jpg) 
5. Click Save 
 
You can create config Rules to monitor a number of items within your infrastructure. Beside utilizing AWS managed Config rules you can also create custom rules using Lambda Functions. Located here in [Github](https://github.com/awslabs/aws-config-rules) are same sample config rules you can create and implement in AWS Lambda. 
 
#### **Deploy an EC2 Instance** 
 
Let's Deploy and EC2 Instance to test our AWS Confifg rule.  
  - You Can Deploy via **Web Console** or  you can run the following Command from the AWS CLI. If launching via the Web Console, launch a T2.Micro Amazon Linux instance.  Notice that we are not assigning an IAM role to the instance.  
``` 
aws ec2 run-instances --image-id $(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --query 'Parameters[0].[Value]' --output text) --count 1 --instance-type t3.large --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=configruletest}]' 
``` 
Go back to the AWS Config Rule and click into the Rule and run Re-Evaluate after the EC2 is deployed and Running. You will have wait for the result and refresh the web page. However, the instance we deployed should be flagged as non-compliant. 
 
![](../images/reevaluatessmrule.jpg) 
 
Let's Remediate this by adding a remediation action 
 
1. Go back to AWS Config, and edit the ec2-instance-managed-by-systems-manager config rule. Lets set a Remediation Action to attach the needed IAM Role. Under Choose remediation Action do the following: 
    - Remediation action: *AWS-AttachIAMToInstance* 
    - Resource ID parameter: *InstanceId* 
    - This passes the Non-Compliant Instance ID to the Remediation Action 
    - Get the Role Name from the output of the Cloudformation stack Named EC2RoleName and paste it here. 
2. Click Save 
 ![](../images/ssmremediation.jpg) 
3. Go back into the AWS Config rule and take a look at Noncompliant resources. Select the Instance we deployed and then Click on Remediate. 
4. Go to AWS Systems Manager, Click on Automation on the left side. You should see a Automation Task Kick off, that will attach the Role to the Instance. 
5. Once Completed, Reboot the Instance to quicken the process 
6. Go Back AWS Systems Manager, and check under Managed Instances. When the instance shows up as a managed Instance, re-evaluate the rule AWS-Config Rule. 
 
What did we learn? 
 
  - How to create an AWS Config Rule to evaluate if Instances are managed by SSM 
  - Importance of having the right IAM Role assigned for the Instance to Report to SSM. 
  - How to use AWS Systems Manager Automation Documents to Remediate Non-Compliant Instances 
 
#### **Creating a Config Rule To make sure CloudTrail is Enabled** 
 
In this step we will create a config rule using an AWS Managed rule that will evaluate whether Cloudtrail is enabled within your AWS Account. 
 
1. Let's go to the AWS Config Console, once there click on Rules in the left side of the console. 
2. Click on **Add Rule** 
3. In the Add Rule Screen in the Filter section type *cloudtrail-enabled*, click on the cloudtrail-enabled rule. 
4. Under Trigger the Trigger Section, notice the Trigger type is Periodic.  
    - Change the Frequency to 1 hour 
5. Under Rule Parameters 
    - S3BucketName: *management-tools-week-\(**Today'sDate**\)-\(**yourcellnumber**\)-* 
    - cloudWatchLogsLogGroupArn:  
    ``` 
    arn:aws:logs:<Region>:<AccountID>:log-group:ManagementToolsWeek/CloudTrail:* 
    ``` 
6. Click Save 
 
 ![](../images/cloudtrailconfigrule.jpg) 
 
 
 
#### Set Triggers for Lambda Functions
 
Now we will create the triggers for the Lambda Function deployed by the Cfn.  
**Note:** This step needs to be done correctly for the Lambda to Trigger.  
 
1. Go to CloudWatch Console, and Under Events on the left side click on Rules 
    - Click Create rule 
    - Under Event Source 
      - Select the radio button next to Event Pattern 
      - Service Name: Config 
      - Event Type: Config Rules Compliance Change 
      - Select the radio button next to **Specific message type** 
        - From the Drop Down Select **ComplianceChangeNotification** 
      - Select radio button next to **Specific rule name** 
        - Type cloudtrail-enabled 
    - Under Targets 
      - Select the ConfigSSMLab-EnforceCloudTrailFunction\* Lambda Function, which is the function deployed by the CloudFormation. Feel Free to take a look at the function code in Lambda.  
 ![](../images/cwevtlambdatrigger.jpg) 
2. Click Configure details 
    - Configure rule details 
      - Name: CloudTrailChange 
      - State: Check Enabled Box 
      - Click Create Rule 
 
#### Testing our enforce cloudtrail Lambda
 
1. Go to the trail we setup in the first lab, and remove the **CloudWatch Logs Configuration** by clicking on the trash can as the next screenshot points to. 
 ![](../images/removecwlog.jpg) 
2. Go to our Config rule for Cloudtrail, and re-evaluate the rule. Refresh the screen and make sure it comes up as Noncompliant. 
3. Go Back to Cloudtrail, Did the CloudWatch Log Configuration return? Did you Get an E-mail?  
 
What did we learn? 
 
  - How to use CloudWatch Events to automatically Trigger Lambda Functions to automatically Remediate Non-Compliant Resources 
  - Multiple ways to Automate and Remediate Resources that Drift within AWS. 
 
#### Ensure CloudWatch is Installed on Instances
 
In this step we will create a State Manager job that will run on a schedule to make sure the latest version of the CloudWatch Agent is installed on our instance. System Manager State Manager is a secure and scalable configuration management service that ensures your Amazon EC2 and hybrid infrastructure is in an intended or consistent state, which you define. 
 
1. Go to the AWS System Manager Console, on the left side under *Actions* click on *State Manager*. 
2. Click on **Create Association** 
3. Provide a name for your association – CloudWatchAgentInstall 
4. Under Command Document 
      - Click the radio button next to *AWS-ConfigureAWSPackage* 
5. Under Parameters 
      - Action: Install 
      - Name: AmazonCloudWatchAgent 
      - Version: latest 
6. Under Targets, Manually check the Instance deployed earlier. 
7. Under Specify Schedule 
      - Select Radio Button next to CRON schedule builder 
      - Every Day at 22:30 
8. Click on **Create Association** 
 
This association will run every day at 10:30 PM and make sure the latest version of the CloudWatch Agent is Installed. We can then run another Association to Pull down the Configuration from the Parameter Store 
 
What did we learn? 
 
  - How to use AWS Systems Manager State Manager to make sure that CloudWatch Agent is installed and up to date. 
  - We can use AWS Systems Manager State Manager to ensure a certain state of our EC2 Instances 
 
 
#### Observer Configuration Timeline and Compliance Timeline
 
**Observe Instance Configuration Timeline** 
 
1. Go to AWS Config, click on Resources 
2. Select EC2: Instance, and then click Lookup 
3. Click into an Instance we deployed in this lab, then Click on Configuration timeline 
![](../images/configtimeline.jpg) 
4. Observe the Timeline, and the changes that occurred 
![](../images/ConfigConfigurationTimeline.jpg) 
5. Then click on Managed instance information in the right hand corner to see the integration data between AWS Config and AWS Systems Manager.  
 
**Observe CloudTrail Compliance Timeline** 
 
1. Go to AWS Config, click on Resources 
    - If you are already within the Instance Click on Compliance timeline 
![](../images/ComplianceTimeline1.jpg) 
2. Select EC2: Instance, and then click Lookup 
3. Click into an EC2 Instance we deployed in this lab, then Click on Compliance timeline 
![](../images/ComplianceTimeline.jpg) 
4. Observe the Timeline, and the compliance changes that occurred 
 
**ManagementToolsWeek/CloudTrail in CloudWatch Logs Insight** 
 
1. Navigate to the CloudWatch Logs Console, on the left side Click on **Insight** 
2. Choose the **Log Group** you want to work with from the pull down, at the top right of the grey field. Choose *ManagementToolsWeek/CloudTrail* 
3. Choose the pull down in the time window definition box, at the top right of the grey field. 
4. Choose to define a Relative time for the past two days.  
5. Select Sample Queries --> CloudTrail --> Number of log entries by service, event type, and region 
``` 
stats count(*) by eventSource, eventName, awsRegion 
``` 
6. Run Query and Observe and play around with the Data and Graph.  
![CW Insights ManagementToolsWeek/CloudTrail](../images/cwicloudtrail.jpg) 

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 