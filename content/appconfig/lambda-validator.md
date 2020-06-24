---
weight: 13
title: "Lambda Validator"
---

Lambda Validators provide a greater level of flexibility than JSON Schema Validators given you have the freedom to write code to perform whatever level of validation you want on your Configuration Document. JSON Schema Validators are limited to the valdations specified in the JSON Schema version 4.x specification.

In this section of the lab, we will perform a simple data type validation on one of the properties in our configuration document - `intResultLimit`.  We will create a Lambda to use for validation, assign AppConfig permissions to invoke the Lambda when AppConfig deployment is initiated and then test our Lambda Validation.  Our tests will include first a negative test so that you can observe the behavior when the Lambda Validation fails and a second successful test that allows the deployment to continue.

#### Create a Lambda Validator

Use the following procedure to create a Lambda Validator.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **Create function**.

2. Choose the **Author from scratch** option.

3. Enter **AppConfigLabLambdaValidator** in the **Function name** field.

4. Choose **Node.js 12.x** from the **Runtime** dropdown list.

5. Expand the **Permissions** section and choose the option to **Create a new role with basic Lambda permissions** from the **Execution role** options.![](../images/appconfig-create-api-gateway-lambda.png)

6. Select **Create function**.


#### Create Resource-based policy to allow AppConfig to Invoke Lambda

In order for AppConfig to be able to invoke the Lambda Validator, we must first create a resource-based policy to specifically allow the AppConfig service to invoke the **AppConfigLabLambdaValidator** we just created.  We will do this through the AWS CLI using the `aws lambda add-permission` command.

Use the following procedure to create a resource-based policy.

1. Open terminal on your local machine.

2. Execute the following AWS CLI command.

    ```
    aws lambda add-permission --function-name AppConfigLabLambdaValidator --action lambda:InvokeFunction --statement-id appconfig --principal appconfig.amazonaws.com --output json --region us-east-1
    ```

    Note that we are granting permissions for the AppConfig service to invoke the **AppConfigLabLambdaValidator** specifically.
    
3. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabLambdaValidator** from the functions list.

4. Select the **Permissions** tab and scroll down to the **Resource-based policy** section.  It should resemble the following: ![](../images/appconfig-lambda-validator-policy.png?width=60pc)


#### Add Validation Code to Lambda Validator

Use the following procedure to add validation code to the Lambda Validator.

1. Select the **Configuration** tab.

2. Replace the entire contents of the **Function code** field with the following:

    ```
    exports.handler = async (event, context) => {
      try {
        console.log('Received event:', JSON.stringify(event, null, 2));
        /* Received event:
        {
          "content": "ewogICJib29sRW5hYmxlTGltaXRSZXN1bHRzIjogZmFsc2UsCiAgImludFJlc3VsdExpbWl0IjogMCwKICAiYm9vbEVuYWJsZUZpbHRlcmluZyI6IHRydWUKfQ==",
          "uri": "arn:aws:lambda:us-east-1:xxxxx:function:AppConfigLabLambdaValidator"
        }
        */
        
        const data = JSON.parse(Buffer.from(event.content, 'base64').toString('ascii'));
        console.log('Configuration Data: ', data);
        /* Configuration data
        {
          boolEnableLimitResults: false,
          intResultLimit: 0
        }
        */
        
        Object.keys(data).forEach(function (item) {
          console.log('key: ',item); // key
          console.log('value: ', data[item]); // value
          const dataType = typeof data[item];
          console.log('Data type: ', dataType);
          
          if (item === 'intResultLimit' && dataType !== 'string')
            throw new TypeError(`Configuration property ${item} configured as type ${dataType}; expecting type string`);
        });
      } catch(err) {
        throw err;
      }
    };
    ```

    Note that the code above **incorrectly** attempts to validate the `intResultLimit` property as a type `string`.  This is intentional and will allow us to perform a negative test of our code initially as well as observe in CloudWatch logs the `console.log` output to demonstrate the structure of the event received by the lambda from AppConfig.

3. Select **Save**.

#### Add Lambda Validator to AppConfig Configuration Profile

In order to utilize the Lambda to validate our AppConfig Configuration Document, we will need to add a Lambda Validator to our Configuration Profile.

Use the following procedure to add a Lambda Validator to the AppConfig Configuration Profile.

1. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) Console and select **AppConfigLab** from the **Applications** list.

2. Select the **Configuration profiles** tab and select **AppConfigS3ConfigurationProfile** from the **Configration profiles** list.

3. Select the **Actions** dropdown list and choose **Update configuration profile**.

4. Select **Add validator** and expand the **Validator 2** section.

5. Select **AWS Lambda** and then select **AppConfigLabLambdaValidator** from the **Lamdba function** dropdown list.

6. **Important!** Do not make a selection from the **Function version** dropdown list.  Leave it blank.

7. Select **Update configuration profile**.

#### Deploy Updated Configuration Profile & Negative Test Lambda Validator

Next, we will deploy our updated configuration profile that now has the Lambda Validator attached.  Remember, our Lambda function code has a check within it to validate that the `intResultLimit` property is a type `string`.  In our configuration document, the `intResultLimit` property is set to `1` as a number type.  When we attempt to deploy our function, the deployment will fail as a result.

Use the following procedure to redeploy our configuration profile to development and negative test the Lambda Validator.

1. Select **Start deployment**.

2. Select **AppConfigLabLambdaDevelopment** from the **Environment** dropdown list.

3. Select **AppConfigLabLinearTwentyPercentDeploymentStrategy** from the **Deployment strategy** dropdown list.

4. Enter **Negative test Lambda Validator** in the **Deployment description** field.

5. Select **Start deployment** and observe a validation error is thrown. 
![](../images/appconfig-lambda-validator-error.png?width=50pc)


#### Update Lambda Code & Perform Successful Deployment

Next, we will correct our Lambda Validator code so that the deployment will succeed and redeploy.

Use the following procedures to update the Lambda code and redeploy.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabLambdaValidator** from the **Functions** list.

2. Select the **Configuration** tab and locate the following line of code in the **Function code** section.

    ```
    if (item === 'intResultLimit' && dataType !== 'string')
    ```

3. Replace this line of code with the following which correctly checks that `intResultLimit` is a `number` type.

    ```
    if (item === 'intResultLimit' && dataType !== 'number')
    ```

4. Select **Save**.

4. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) Console and select **AppConfigLab** from the **Applications** list.

6. Select the **Configuration profiles** tab and select **AppConfigS3ConfigurationProfile** from the **Configration profiles** list.

7. Select **Start deployment**.

8. Select **AppConfigLabLambdaDevelopment** from the **Environment** dropdown list.

9. Select **AppConfigLabLinearTwentyPercentDeploymentStrategy** from the **Deployment strategy** dropdown list.

10. Enter **Test Lambda Validator** in the **Deployment description** field.

11. Select **Start deployment** and observe the Percentage complete as it proceeds to 100% and note the transition of the **State** field to **Baking** and eventually to **Completed**.
