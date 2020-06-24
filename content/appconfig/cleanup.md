---
weight: 17
title: "Cleanup"
---

Use the following procedures to delete the assets that were created during this lab.

#### Delete CloudWatch Alarm

1. Visit the [CloudWatch](https://console.aws.amazon.com/cloudwatch/) console and choose **ALARM** under **Alarms**.

2. Select the checkbox next to the **AppConfig-API-5XX-Alarm** alarm from the **Alarms** list.

3. Select **Delete** from the **Actions** dropdown list.



#### Delete Lambdas

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select the radio button next to the **AppConfigLabAPIGatewayLambda** in the functions list.

2. Select **Delete** from the **Actions** dropdown list.

3. Repeat these steps and delete the **AppConfigLabLambdaValidator** Lambda.



#### Delete API Gateway

1. Visit [API Gateway](https://console.aws.amazon.com/apigateway/) and select the radio button next to the **AppConfigLabAPI** API from the **APIs** list.

2. Select **Delete** from the **Actions** dropdown list.



#### Delete AppConfig Application, Configuration Profile and Environment

1. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) Console and select **AppConfigLab** from the **Applications** list.

2. Select **Delete environment** to delete the **AppConfigLabLambdaDevelopment** environment and then select **Delete** from the **Delete environment** confirmation popup.

3. Select **Configuration profiles** tab.

4. Select **Delete configuration profile** to delete the **AppConfigLabS3ConfigurationProfile** configuration profile and then select **Delete** from the **Delete configuration profile** confirmation popup.

4. Select **Delete application** to delete the **AppConfigLab** application and then select **Delete** from the **Delete application** confirmation popup.


#### Delete IAM Policies & Roles

1. Visit the [IAM Console](https://console.aws.amazon.com/iam) and select **Policies**.

2. Enter **AppConfig** in the **Search** field.

3. Select the radio button next to the **AppConfigLabCloudWatchPolicy** and then select **Delete** from the **Policy actions** dropdown list.

4. Select **Delete** from the **Delete policy** confirmation popup.

5. Repeat these steps to delete all the Customer managed polcies created during this lab.

6. Select **Roles** and then enter **AppConfig** in the **Search** field.

7. Select the checkboxes next to all the roles returned from the search and then select **Delete role**. 

#### Delete S3 Bucket

1. Visit the [S3](https://s3.console.aws.amazon.com/s3) console and select the radio button next to the S3 bucket you created.

2. Select **Delete** and then click on the **empty bucket configuration** link in the **This bucket is not empty** error message.

3. Enter the bucket name in the field provided and select **Empty**.

4. Select **Exit**

5. Select **Delete**, enter the name of the bucket to confirm deletion and then select **Delete bucket**.