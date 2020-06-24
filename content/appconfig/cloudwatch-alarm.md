---
weight: 15
title: "Setup CloudWatch Alarm"
---
In this section of the lab, we are going to setup a CloudWatch Alarm to monitor our API for 5XX errors and demonstrate that AppConfig will rollback deployment when an alarm associated with our AppConfig Environment triggers.


#### Deploy API Gateway

Before we begin we will need to create a development stage and deploy our API Gateway so that we can monitor it for 5XX Errors in CloudWatch.  

Use the following procedures to create a deployment and stage for API Gateway.

1. Visit the [API Gateway](https://console.aws.amazon.com/apigateway/) Console and locate the **AppConfigLabAPI** row in the list.

2. Make note of the value for the ID column for the **AppConfigLabAPI** row. ![](../images/appconfig-api-list.png?width=70pc)

3. Using the ID obtained in the previous step, execute the following AWS CLI command to create a deployment and stage.

```
aws apigateway create-deployment --rest-api-id YOUR_API_ID --region us-east-1 --stage-name development
```

4. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the list of functions.

5. Select the **Configuration** tab.

6. In the **Designer** section, locate the **API Gateway** block and select it in order to display the **API Gateway** section.

7. Locate the **API endpoint** URL in the **API Gateway** section.  

8. Click the **API endpoint** URL.  This will execute the API in the stage we configured - development - and is necessary to regiter our API in CloudWatch so that we can next setup a CloudWatch Alarm.

#### Configure CloudWatch Alarm Permission Policy for Rollback 

You can configure AWS AppConfig to roll back to a previous version of a configuration in response to one or more Amazon CloudWatch alarms. When you configure a deployment to respond to CloudWatch alarms, you specify an AWS Identity and Access Management (IAM) role. AppConfig requires this role so that it can monitor CloudWatch alarms even if those alarms weren't created in the current AWS account.

Use the following procedure to create an IAM policy that gives AppConfig permission to call the **DescribeAlarms** API action.

1. Visit the [IAM Console](https://console.aws.amazon.com/iam) and select **Policies**.

2. Select **Create Policy**.

3. On the **Create Policy** page, select the **JSON** tab.

4. Replace the default content on the JSON tab with the following permission policy, and then choose **Review policy**.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "cloudwatch:DescribeAlarms"
        ],
        "Resource": "*"
    }]
}
```

5. On the **Review** page, enter **AppConfigLabCloudWatchPolicy** in the **Name** field.

6. Select **Create policy** to complete the process and return to the **Policies** page.


#### Create IAM role for the rollback based on CloudWatch alarms 

Use the following procedure to create an IAM role and assign the policy you created in the previous procedure to it.

1. Visit the [IAM Console](https://console.aws.amazon.com/iam) and select **Roles** and then choose **Create role**.

2. Under **Select type of trusted entity**, choose **AWS Service**.

3. Immediately under **Choose a use case**, choose **EC2**, and then choose **Next: Permissions**.

4. On the **Attached permissions policy** page, search for **AppConfigLabCloudWatchPolicy**.

5. Choose this policy and then choose ***Next: Tags***.

6. Enter tags for this role (optional), and then choose ***Next: Review***.

7. On the **Create role** page, enter the name **AppConfigLabCloudWatchRole** in the **Role name** field, and then update the Role description to **Allows role to call CloudWatch DescribeAlarms**.

8. Then, choose **Create Role**.



#### Add Trust Relationship for AppConfig to Assume Role 

Use the following procedure to assign a trust relationship which allows AppConfig to assume the role that you just created and call CloudWatch DescribeAlarms API action.

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**.

2. Edit the policy to include only **“appconfig.amazonaws.com”**, as shown in the following example:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "appconfig.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
3. Choose **Update Trust Policy**.



#### Create a CloudWatch Alarm to Monitor API Gateway 5XX Errors

Use the following procedure to create a CloudWatch alarm that will enable AppConfig to roll back a configuration when the alarm is triggered.

1. Visit the [CloudWatch](https://console.aws.amazon.com/cloudwatch/) console and choose **ALARM** under **Alarms**.

2. Choose **Create alarm**.

3. Choose **Select metric** and then **ApiGateway** from the **AWS Namespaces** section.

4. Select the **By Api Name** option and then select the row with **Metric Name** indicates as **5XXError** for **ApiName** indicated as **AppConfigLabAPI**.

5. Select **Select metric** to continue.

6. Change **Statistic** to **Sum** and enter **5** as the threshhold value. ![](../images/appconfig-cloudwatch-alarm.png?width=50pc)

7. Select **Next**.

8. On the next page, **Configure actions**, select **Remove** from the **Notification** section and then select **Next**.  We will not create a notification action for this example during the lab.

9. Enter **AppConfig-API-5XX-Alarm** in the **Name** field and enter an optional **Alarm description**.

10. Select **Next** and then select **Create alarm**.


#### Update Lambda Configuration to Return 5XX Error to API Gateway

The quickest way to force API Gateway to return a 500 Internal Server Error and trigger the **AppConfig-API-5XX-Alarm** CloudWatch Alarm, is to simply remove the API Gateway trigger directly from the **AppConfigLabAPIGatewayLambda** Lambda.

Use the following procedure to remove API Gateway trigger from Lambda.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the functions list.

2. Select the **Configuration** tab and Select API Gateway in the **Designer** section.  

3. Select **Delete** from the **API Gateway section**.![](../images/appconfig-lambda-remove-trigger.png?width=70pc)

4. Select **Save**.


Now that we have deleted the association between API Gateway and Lambda, any call to our API Gateway will throw a 500 Internal Server Error and trigger our CloudWatch Alarm provided the thresshold that we configured (i.e., >= 5 errors within a 5 minute period).


#### Update AppConfig Environment to Include CloudWatch Alarm

Next, we need to add the CloudWatch Alarm to our AppConfig Environment configuration so that AppConfig can monitor the alarm during deployment.

Use the following procedure to attach a CloudWatch Alarm to the AppConfig Environment.

1. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) console and select **AppConfigLab** from the **Applications** list.

2. Select **AppConfigLabLambdaDevelopment** from the **Environments** list and then select **Update environment**.

3. Expand the **Monitors** section and enter **AppConfigLabCloudWatchRole** in the **IAM role** field.

4. Select **AppConfig-API-5XX-Alarm** from the **Cloudwatch alarms** dropdown list.

5. Select **Add** then select **Update environment**.


#### Redeploy AppConfig Environment

1. Select **Start deployment**.

2. Select **AppConfigLabS3ConfigurationProfile** from the **Configuration** dropdown list.

3. Select **Create deployment strategy**.  We will create a new deployment strategy that will have a longer deployment and bake time to allow us enough time to generate errors on API gateway and observe that as our API Gateway 5XX alarm is triggered, AppConfig will rollback the deployment.

4. Enter **AppConfigLabTwentyMinuteDeploymentStrategy** in the **Name** field, accept the remaining defaults of a Linear deployment type, 20 step percentage, 10 minute deployment time and 10 minute bake time.  Overall, the deployment should take 20 minutes. 

5. Select **Create deployment strategy**. 

6. Reselect **AppConfigLabS3ConfigurationProfile** from the **Configuration** dropdown list, if your previous selection cleared out.

7. Select **Start deployment**


#### Generate API Gateway 5XX Errors and Trigger Deployment Rollback

1. Visit the [API Gateway](https://console.aws.amazon.com/apigateway/) Console and select **AppConfigLabAPI** from the **APIs** list.

2. Select **Stages** from the left navigation and then select **development** to revel the invocation URL for this stage of the **AppConfigLabAPI**.

3. Click on the Invoke URL.  A new browser window will open and it should display `{"message": "Internal server error"}`.  Refresh this page several times to generate enough errors to trigger the CloudWatch Alarm.  

4. In a new browser window, visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) console and select **AppConfigLab** from the **Applications** list.

5. Select **AppConfigLabLambdaDevelopment** from the **Environments** list and then select the latest **Deployment number** from the **Deployments list**.

6. Observe that the deployment was rolled back due to the CloudWatch Alarm being triggered. ![](../images/appconfig-deployment-rollback.png?width=70pc)

...
