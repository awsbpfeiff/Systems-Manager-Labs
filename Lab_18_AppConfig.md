

In this lab, we will go over the topics that describe how to use AWS AppConfig features to create and deploy a configuration to your hosts or targets.

### Step 1: Create an application

1. Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2. In the navigation pane choose **AppConfig**.

3. (If this is the first time using AppConfig) - Select **Create configuration data** under **Start using AppConfig** from the right.
(If this is not the first time using AppConfig) - On the **Applications** tab, choose **Create application**.

4. For **Name**, enter a name for the application.

5. For **Description**, enter information about the application.

6. In the **Tags** section, enter a key and an optional value. You can specify a maximum of 50 tags for a resource.

7. Choose **Create application**.

### Step 2: Create an environment
For each AWS AppConfig application, you define one or more environments. An environment is a logical deployment group of AppConfig targets, such as applications in a Beta or Production environment. You can also define environments for application subcomponents such as the Web, Mobile, and Back-end components for your application. You can configure Amazon CloudWatch alarms for each environment. The system monitors alarms during a configuration deployment. If an alarm is triggered, the system rolls back the configuration.

1. Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2. In the navigation pane choose **AppConfig**.

3. On the **Applications** tab, choose the application you created in earlier section and then choose **View details**.

4. On the **Environments** tab, choose **Create environment**.

5. For **Name**, enter a name for the environment.

6. For **Description**, enter information about the environment.

7. Leave the **Monitors** section, as is. This is used for AppConfig to roll back a configuration when a CloudWatch Alarm is triggered. We are not going to cover that in this lab.

8. In the **Tags** section, enter a key and an optional value. You can specify a maximum of 50 tags for a resource.

9. Choose **Create environment**.

### Step 3: Create a configuration and configuration profile
A configuration is a collection of settings that influence the behavior of your application. For example, you can create and deploy configurations that carefully introduce changes to your application or turn on new features that require a timely deployment, such as a product launch or announcement.

A configuration profile enables AWS AppConfig to access your configuration in its stored location. You can store configurations in one of the following locations:

- Objects in an Amazon Simple Storage Service (Amazon S3) bucket
- Documents in the Systems Manager document store
- Parameters in Parameter Store

A configuration profile includes the following information.

- The URI location where the configuration is stored.
- The AWS Identity and Access Management (IAM) role that provides access to the configuration.
- A validator for the configuration data. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile. A configuration profile can have a maximum of two validators.

First, we will create configuration object as a parameter in Parameter STORE_USER_REQUEST_INSERT_SQL
1. Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2. In the navigation pane choose **Parameter Store**. Select **Create parameter**

3. For **Name**, enter **/demoapp/devenv/user_accesss_configration**.

4. For **Tier**, select **Standard**.

5. For **Type**, select **String**.

6. Select **text** in the **Data type** drop down.

7. Enter following in the **Value** field:

{
    "UserAccessList": [
        {
            "user_name": "Jane_Doe"
        },
        {
            "user_name": "Mike_Smith"
        }
    ]
}

8. Click **Create parameter**.

Next, we will create the configuration profile using the above configuration object in AppConfig.


9. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

10. On the **Applications** tab, from **AppConfig**, choose the application you created in above section and then choose the **Configuration profiles** tab\.

11. Choose **Create configuration profile**\.

12. For **Name**, enter a name for the configuration profile\.

13. For **Description**, enter information about the configuration profile\.

14. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\.

15. On the **Select configuration source** page, choose **AWS Systems Manager parameter**\.  
![\[Choose an AWS AppConfig configuration source\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-1.png) then complete the following steps\.

   16. In the **Parameter** section, choose the parameter created above. Click **Next**.

   17. In **Service role** section, select **New service role** and keep the default name in the **Role name** field.

   18. Skip the **Validator** section. **Note** SSM parameters do not require a validation method, but we recommend that you create a validation check for new or updated SSM parameter configurations by using AWS Lambda\.

   19. Click **Create configuration profile**\.This operation may take up to thirty seconds. Please be patient while it completes.

### Step 4:  Create a deployment strategy

1. Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2. In the navigation pane, choose **AppConfig**.

3. Choose the **Deployment Strategies** tab, and then choose **Create deployment strategy**.

4. For **Name**, enter a name for the deployment strategy.

5. For **Description**, enter information about the deployment strategy.

6. For **Deployment type**, choose **Linear**.

7. For **Step percentage**, set the percentage of callers to target during each step of the deployment to **20**.

8. For **Deployment time**, enter the total duration for the deployment in minutes or hours. Set it to **2 minutes**.

9. For **Bake time**, enter the total time, in minutes or hours, to monitor for Amazon CloudWatch alarms before proceeding to the next step of a deployment or before considering the deployment to be complete. Set it to **2 minutes**.

10. In the **Tags** section, enter a key and an optional value. You can specify a maximum of 50 tags for a resource.

11. Choose **Create deployment strategy**

### Step 5: Deploy a configuration
1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **AppConfig**\.

1. On the **Applications** tab, choose application created above, and then choose **View details**\.

1. On the **Environments** tab, choose an environment, and then choose **View details**\.

1. Choose **Start deployment**\.

1. For **Configuration**, choose configuration created above, from the list\.

1. Use the **Parameter version** list to choose the version you want to deploy\.

1. For **Deployment strategy**, choose a strategy created above, from the list\.

1. For **Deployment description**, enter a description\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\.

1. Choose **Start deployment**\. Wait until the deployment is finished.

### Step 6: Receiving a configuration

In this section, we will use **GetConfiguration** API that clients will need to use to pull configuration from AppConfig.
**Note**: You may need to upgrade the AWS CLI to the latest version

1. Execute following AWS CLI command, noting the details from the above sections :

aws appconfig get-configuration --application <APPLICATION_NAME_IN_APPCONFIG> --environment <ENV_NAME_IN_APPCONFIG> --configuration <CONFIG_NAME_IN_APPCONFIG> --client-id <ANY_UNIQUE_NAME> --region <AWS_REGION> outfile > output.txt

2. Check the output by opening the file. It will show the version that was pulled from the AppConfig:

{
    "ContentType": "application/octet-stream",
    "ConfigurationVersion": "1"
}

3. Update the configuration object in the Parameter Store with following content:

{
    "UserAccessList": [
        {
            "user_name": "Jane_Doe"
        },
        {
            "user_name": "Mike_Smith"
        },
        {
            "user_name": "John_Doe"
        }
    ]
}

4. Navigate to AWS Systems Manager -> AppConfig -> <Name of your application> -> <Name of your configuration profile>. It will reflect two versions of the configuration object from the parameter store. Select the latest version, click **Start Deployment** on the top right and follow the steps mentioned above on deploying a configuration.


5. When the deployment completes, execute **GetConfiguration** command, as stated in step 1 and check the output of the file. It should reflect second version of the configuration.

{
    "ContentType": "application/octet-stream",
    "ConfigurationVersion": "2"
}
