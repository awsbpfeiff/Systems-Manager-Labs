---
weight: 11
title: "Feature Toggle"
---

In this section of the lab, we will modify our API Gateway Lambda to utilize the `boolEnableLimitResults` and `intResultLimit` properties of our configuration document to enable a new feature that will limit the number of results returned by API Gateway as well as utilize the value of `intResultLimit` to determine how many results to return.

The current state of our configuration document is as follows:

```json
{
   "boolEnableLimitResults": false,
   "intResultLimit": 0
}
``` 

We will first need to modify our Lambda code to implement the limit feature, update our configuration document to turn on the feature and then redeploy our configuration profile to development.



#### Add Feature Toggle Code to Lambda to Enable Limiting

Next, we will update our Lambda application to implement the `boolEnableLimitResults` configuration value to turn on the limit results feature code block as well as utilize the value of `intResultLimit` to determine the number of results returned in the API response.

Use the following procedure to implement the limit feature toggle.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the functions list.

2. Select the **Configuration** tab and scroll down to the **Function code** section.

3. Locate the following line of code: `// add result limit code here`.

4. Replace this entire line with the code below.  This block of code first evaluates if the value for **boolEnableLimitResults** is present and set to *true*. It then uses the value of **intResultLimit** to control the number of results returned.

    ```
    const parsedConfigData = JSON.parse(configData);
    
    if ( (parsedConfigData.boolEnableLimitResults) && parsedConfigData.intResultLimit ) {
      result = result.splice(0, parsedConfigData.intResultLimit);
    }
    ```


5. Locate the line of code *body: parsedConfigData* and replace it with *body: result* such that the response constant looks reads as follows:

    ```
    const response = {
      statusCode: 200,
      body: result,
    };
    ```

6. Select **Save**.

7. Test the API Gateway by returning to the [API Gateway](https://console.aws.amazon.com/apigateway/) Console and select **AppConfigLabAPI** from the list.

8. Select the **GET** method from the **/** root Resources.

9. Select **Test** and note that all the items in the array are returned.

Note that the limiting feature did not work because the value for **boolEnableLimitResults** is set to *false*.  We need to update **AppConfigLabConfigurationDocument.json** and change the value of **boolEnableLimitResults** to *true* to enable limiting as well as change the value of **intResultLimit** to *5* from *0* so that we get results.  Then, we will need to redeploy AppConfig to our development environment to allow the API Gateway Lambda to access the updated configuration data.



#### Update & Deploy AppConfig Configuration Document

Use the following procedure to update the configuration data and redeploy it to the development environment.

1. Open your local copy of the **AppConfigLabConfigurationDocument.json** file and replace the contents of the file with the content below and then save.

    ```
    {
      "boolEnableLimitResults": true,
      "intResultLimit": 5
    }
    ```

2. Visit the [S3](https://s3.console.aws.amazon.com/s3) console and locate the bucket you stored your configuration data within.  It should be named something like *appconfig-lab-firstname-lastname*.

3. Select the S3 bucket and then select **Upload**.  Complete the upload wizard to upload the latest version of **AppConfigLabConfigurationDocument.json** from your local machine.

4. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) Console and select the **AppConfigLab** application from the **Applications** list.

5. Select the **AppConfigLabLambdaDevelopment** environment from the **Environments** list.

6. Select **Start deployment**.

7. Select **AppConfigLabS3ConfigurationProfile** from the **Configuration** dropdown list and then choose **AppConfigLabLinearTwentyPercentDeploymentStrategy** from the **Deployment strategy** dropdown list.

8. Enter **Deploy limiting feature enabled** in the **Deployment description** field.

9. Select **Start deployment** to being the deployment.

10. Observe the **Percent complete** and **State** until it reaches **100%** and a state of **Completed**.  Refresh the page after a couple of minutes to make sure the status updates.

With the updated configuration deployed, `"boolEnableLimitResults": true` and `"intResultLimit": 5` are now both available for use by the API Gateway application.



#### Verify Feature Toggle in API Gateway

Next, we will verify that the limit feature is now in fact enabled.

1. Test the API Gateway by returning to the [API Gateway](https://console.aws.amazon.com/apigateway/) Console and select **AppConfigLabAPI** from the list.

2. Select the **GET** method from the **/** root Resources.

3. Select **Test** and note that only five items returned within the API response.  The response should resemble the following:

```json
{
  "statusCode": 200,
  "body": [
    {
      "name": "AWS AppConfig"
    },
    {
      "name": "Amazon SageMaker Studio"
    },
    {
      "name": "Amazon Kendra"
    },
    {
      "name": "Amazon CodeGuru"
    },
    {
      "name": "Amazon Fraud Detector"
    }
  ]
}
```
