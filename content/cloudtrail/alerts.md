---
weight: 2
title: "Alerting"
---

**Note :** Make sure complete the Setup before continuing with this section.

#### Creating Alerts via CloudWatch Events

In this section, we will use CloudWatch Events to monitor and alert when an IAM policy is attached to an IAM user.  The example is simple and it helps to depict the level of visility that can be gained from using this type of process.  The CloudWatch Event Rule created will monitor for a specific event name in CloudTrail, and will use an SNS message to notify regarding and event, when it occurs.

1. Go to the [CloudWatch Events Rules console](https://console.aws.amazon.com/cloudwatch/home#rules:).
2. Click on the Create rule button.
3. Configure the Rule using the following settings:
 - Event Pattern: Build event pattern to match events by service
 - Service Name: IAM
 - Event Type: AWS API Call via CloudTrail
 - Specific operation(s): **AttachUserPolicy**
 - Targets: SNS topic
 - Topic: CloudWatchAlarmsForCloudTrail-AlarmNotificationTopic-XXXXXXXXX

    ![Create CloudWatch Event Rule](../images/CWEventRule.png) 

4. Click Configure details.
5. On the Configure rule details page, enter a Name for the rule (e.g. AttachUserPolcity_CWE).
6. Click Create Rule.

    ![CloudWatch Event Rule Create Success](../images/CWEventRule.png) 


#### Triggering AttachUserPolicy Notification
Now that we have an event we are monitoring, we will create an IAM user and attach a user policy to this user to trigger the notification.

1. Go to the [IAM Console](https://console.aws.amazon.com/iam/home#/users).
2. Click on Add user.
3. Set the following values for the user:
 - User name: workshopuser (or any other name)
 - Select AWS access type: AWS Management Console access
 - Console password: Autogeneratted password
 - Requrie password reset: Check the checkbox
4. Click Next: Permissions.
5. On the Set permissions page, we will select:
 - Attach existing policies directly
 - Check the checkbox for the AdministratorAccess policy

    ![Attach User Policy](../images/UserPolicy.png) 

6. Click on Next: Tags.
7. Click Next: Review on the Add tags page.
8. Click Create user.


Once the user is created and the policy has been set, the CloudWatch Event pattern will be triggered and an e-mail will be sent to the e-mail address defined in the setup (i.e. Create CloudWatch Alarms for Security and Network related API activity).

![Policy notification](../images/PolicyNotification.png) 



#### Triggering Network related API Notifications

As part of sending CloudTrail events to CloudWatch Logs, we also deployed a sett of predefined CloudWatch Alarms to monitor Network and Security related API activity.  In this section, we will trigger one of the network related alarms.  Optionally, you can trigger the other [CloudWatch Alarms](https://console.aws.amazon.com/cloudwatch/home#alarmsV2:?~(selectedIds~(~'CloudTrailAuthorizationFailures))) created as part of launching the CloudFormation template in the setup.

1. Go to the [VPC console](https://console.aws.amazon.com/vpc/home#SecurityGroups:sort=groupId).
2. Select the default security group.
3. Right click and select Edit inbound rules.

    ![Edit Security Group](../images/EditSG.png) 

4. Add a rule with the following settings:
 - Type: All ICMP - IPv4
 - Source:Custom
 - Network: 0.0.0.0/0 
5. Click on Save rules.
6. Click Close.

    ![Edit Security Group](../images/SGRule.png) 

Once the alarm is processed, a notificatioin will be sent to the email address configured in the setup.  Review the notification to understand what is logged.

![Security Group Notification](../images/SGNotification.png) 

**End of Lab Exercises** 
 
**Thank you for using this lab.** 