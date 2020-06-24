---
weight: 10
title: "AppConfig & Lambda Integration"
---

At this point we have created a Lambda with an API Gateway trigger that returns our array of JSON objects.  In this section of the lab, we will modify our API Gateway Lambda to utilize the `boolEnableLimitResults` and `intResultLimit` properties of our configuration document to enable a new feature that will limit the number of results returned by API Gateway as well as utilize the value of `intResultLimit` to determine how many results to return.  

We will start with modifying our code to allow us to call the AppConfig GetConfiguration API to retrieve our configuration data and extract from it the boolean value for **boolEnableLimitResults**.  We will use that value as a means to turn on or off our filtering feature.   

Recall, our configuration that we stored in S3 and deployed looks like this:

``` 
{
   "boolEnableLimitResults": false,
   "intResultLimit": 0
} 
```

Note that **boolEnableLimitResults** is set to false.  This is common practice to deploy a feature or block of new code disabled by default.  Once successfully deployed, we can turn it on by changing the value of `boolEnableLimitResults` to `true`, observe for errors and turn it off if we encounter any errors.  This helps avoid a hotfix on production, for example.


#### Modify IAM Lambda Execution Policy to Allow Lambda to Call AppConfig GetConfiguration API

In order to enable our API Gateway Lambda to integrate with AppConfig, we need to add permissions to the Lambda to access our AppConfig Application.

Use the following procedure to add the necessary permissions to the Lambda execution policy to allow our API Gateway Lambda to call the AppConfig GetConfiguration API.

1. Return to the browser tab with your Lambda or Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the list of functions.

2. Select the **Permissions** tab and click on the **Role name** in the **Execution role** section.  This will open in a new browser window or tab our Lambda execution role in the IAM console.

3. In the **Permission policies** section, select the policy attached to this role which should be prefixed with **AWSLambdaBasicExecutionRole-**.

4. Select **Edit policy** and then select the **JSON** tab.

5. Add the following to the policy and replace YOUR_ACCOUNT_NUMBER with your AWS Account number.  If you do not know your Account Number, [click here](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) for instructions on how to obtain it.

```
{
    "Effect": "Allow",
    "Action": "appconfig:GetConfiguration",
    "Resource": "arn:aws:appconfig:us-east-1:YOUR_ACCOUNT_NUMBER:*"
}
```

6. The policy should resemble the following. ![](../images/appconfig-lambda-consumer-policy.png)

7. Select **Review policy** and then **Save changes**.

#### Modify Lambda Code to Call AppConfig GetConfiguration API

Use the following procedure to integrate AppConfig with our API Gateway Lambda.

1. Return to the browser tab with your Lambda or Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the functions list.

2. Select the **Configuration** tab and replace the code in the **Function code** with the following.

    ```
    const AWS = require('aws-sdk');
    const appconfig = new AWS.AppConfig({apiVersion: '2019-10-09'});

    // add params and cache constants code here

    exports.handler = async (event) => {
      
      // add best practices client configuration code here 
       
      const params = {
        Application: 'AppConfigLab', /* required */
        ClientId: 'AppConfigLabAPIGatewayLambda', /* required */
        Configuration: 'AppConfigLabS3ConfigurationProfile', /* required */
        Environment: 'AppConfigLabLambdaDevelopment', /* required */
      };
      
      const appConfigResponse = await appconfig.getConfiguration(params).promise();
      const configData = Buffer.from(appConfigResponse.Content, 'base64').toString('ascii');

      let result = getServices();
          
      // add result limit code here

      const response = {
        statusCode: 200,
        body: JSON.parse(configData),
      };
      return response;
    };

    function getServices() {
      return [
        {
          name: 'AWS AppConfig'
        },
        {
          name: 'Amazon SageMaker Studio'
        },
        {
          name: 'Amazon Kendra'
        },
        {
          name: 'Amazon CodeGuru'
        },
        {
          name: 'Amazon Fraud Detector'
        },
        {
          name: 'Amazon EKS on AWS Fargate'
        },
        {
          name: 'AWS Outposts'
        },
        {
          name: 'AWS Wavelength'
        },
        {
          name: 'AWS Transit Gateway'
        },
        {
          name: 'Amazon Detective'
        }
      ];
    }

    // add create UUID function code here
    ```

3. Select **Save**.

Observe the code and note that we first added code to include the `aws-sdk` and then added some parameters to pass into the AppConfig GetConfiguration API call that reference by name our AppConfig Application, Configuration Profile and Environment.  Additionally, we added a ClientId that is identical to our Lambda Name for tracking purposes.

```
const AWS = require('aws-sdk');
const appconfig = new AWS.AppConfig({apiVersion: '2019-10-09'});

...

const params = {
    Application: 'AppConfigLab', /* required */
    ClientId: 'AppConfigLabAPIGatewayLambda', /* required */
    Configuration: 'AppConfigLabS3ConfigurationProfile', /* required */
    Environment: 'AppConfigLabLambdaDevelopment', /* required */
};
```

We then added code to call the AppConfig GetConfiguration API, read it from the buffer stream and Base64 decode it.

```
  const appConfigResponse = await appconfig.getConfiguration(params).promise();
  const configData = Buffer.from(appConfigResponse.Content, 'base64').toString('ascii');
```

Lastly, we changed the response to actually include the parsed configuration data just to test the integration all the way through.  We can expect that if we perform a test execution of our API Gateway that the body will now contain the actual configuration data we uploaded to S3 instead of a list of AWS Services.

```
const response = {
    statusCode: 200,
    body: JSON.parse(configData)
};
```

Use the following procedure to verify the Lambda can retrieve the configuration.

1. Toggle to your API Gateway browser tab or window or visit [API Gateway](https://console.aws.amazon.com/apigateway/) and select **AppConfigLabAPI** from the list.

2. Select the **GET** method under the root resources, then select **TEST**.

3. Select **Test** and verify that the **Response Body** from the test execution appears as follows.

```json
{
  "statusCode": 200,
  "body": {
    "boolEnableLimitResults": false,
    "intResultLimit": 0
  }
}
```

That's it!  We have now successfully configured the permissions necessary for Lambda to retrieve our AppConfig configuration data and modified our Lambda code to retrieve the data and return it in the body of our response as a means to test the integration between Lambda and AppConfig.

Next up in the lab, we will modify our Lambda code to use the **boolEnableLimitResults** value of our configuration data to enable a new feature that will allow us to the limit the number objects returned by the API.  Additionally, we will use the value of the **intResultLimit** property to configure the number of items to return in the API response.
