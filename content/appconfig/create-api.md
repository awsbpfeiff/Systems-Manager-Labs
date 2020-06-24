---
weight: 9
title: "Create API Gateway Application"
---

Now that we have created and deployed an AppConfig Configuration Document, it is available to applications to consume the configuration data and use it.  

In this section of the lab, we will create an API Gateway and supporting Lambda that will ultimately consume the configuration data we have deployed to the development environment we created.

#### Create AWS Lambda

Use the following procedure to create a Lambda.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **Create function**.

2. Choose the Author from scratch option.

3. Enter **AppConfigLabAPIGatewayLambda** in the **Function name** field.

4. Choose **Node.js 12.x** from the **Runtime** dropdown list.

5. Expand the **Permissions** section and choose the option to **Create a new role with basic Lambda permissions** from the **Execution role** options.![](../images/appconfig-create-api-gateway-lambda.png)

6. Select **Create function**.

7. Replace the contents of the **Function code** with the following code and select **Save**.

    ```javascript
    exports.handler = async (event) => {
      let result = getServices();

      const response = {
        statusCode: 200,
        body: result,
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
    ```

    Note that this block of code simply returns an array of objects (AWS Services) that includes the `name` property.

#### Create AWS API Gateway 

1. Open a new browser tab or window so that you can easily toggle back and forth between your Lambda and API Gateway.

2. In the new browser tab or window, visit [API Gateway](https://console.aws.amazon.com/apigateway/) Console and select **Create API**.

3. Locate the **REST API** with the description **Develop a REST API where you gain complete control over the request and response along with API management capabilities** and select **Build**.

4. Select the **REST** option from the **Choose the protocol** section.

5. Select the **New API** from the **Create new API** section.

6. In the **Settings** section, enter **AppConfigLabAPI** in the **API name** field and an optional **Description**.

7. Select **Regional** from the **Endpoint Type** dropdown list.![](../images/appconfig-create-api-gateway.png)

8. Select **Create API**.

#### Configure API Gateway Lambda Integration

1. Choose the root resource **/** under **Resources**. From the **Actions** menu, choose **Create Method**.

2. Choose **GET** from the **Method** dropdown list and then select the check mark to complete the save operation.

3. Choose **Lambda Function** from the **Integration type** options.

4. Leave **Use Lambda Proxy Integration** unchecked.

5. Choose **us-east-1** from the **Lambda Region** dropdown list.

6. Enter **AppConfigLabAPIGatewayLambda** in the **Lambda Function** field to select the Lambda we created in the previous section and leave **Use Default Timeout** option checked.![](../images/appconfig-api-gateway-create-method.png)

7. Select **Save** and choose **OK** on the **Add Permission to Lambda Function** popup.

8. Select **TEST** and then **Test** again to test the integration between API Gateway and Lambda.  The results **Response Body** should appear as shown below with a **statusCode** value of **200** and the **body** should be an array of an objects each with the **name** property.  ![](../images/appconfig-api-gateway-initial-test.png?width=60%)

At this point in the lab, we have created an API Gateway and integrated it with a Lambda that returns a simple array of JSON objects each of which contains the name property.  Next, we will configure the Lambda to enable AppConfig integration which will include adding permissions for AppConfig to invoke the lambda as well as modifying the code to retrieve the configuration data for use in our Lambda application.



