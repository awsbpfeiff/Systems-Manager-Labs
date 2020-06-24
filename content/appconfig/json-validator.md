---
weight: 12
title: "JSON Validator"
---

JSON Schema Validators define the allowable properties for each application configuration setting and function like a set of rules to ensure that new or updated configuration settings confirm to the best practices required by your application. 

In this exercise, we will implement a JSON Schema Validator as a sort of unit test to make sure that our AppConfig Configuration Document, **AppConfigLabConfigurationDocument.json**, has the properties and data types that we expect.

#### Create a JSON Validator

Use the following procedures to implement a JSON Schema Validator.

1. Visit the [AppConfig](https://console.aws.amazon.com/systems-manager/appconfig) Console and select the **AppConfigLab** application from the **Applications** list.

2. Select the **Configuration profiles** tab and then select **AppConfigLabS3ConfigurationProfile** from the **Configuration profiles** list.

3. From the **Actions** dropdown list, select **Update configuration profile**.

4. Select **Add validator**.

5. Expand **Validator 1** section and select **JSON Schema** in the **Validator type** options.

6. Enter the JSON Schema validation below.  This JSON Schema Validator makes each field required and validates the data type.  Note that it incorrectly indicates **boolEnableLimitResults** as type *string* which will cause our deployment to fail initially so that we can perform a negative test and observe the behavior during a failure.

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "description": "AppConfig Lab JSON Schema Validator example",
      "type": "object",
      "properties": {
        "boolEnableLimitResults": {
          "type": "string"
        },
        "intResultLimit": {
          "type": "number"
        }
      },
      "minProperties": 2,
      "required": [
        "intResultLimit",
        "boolEnableLimitResults"
      ]
    }
    ```

7. Select **Update configuration profile**.

8. Select **Start deployment**.

9. Select **AppConfigLabS3ConfigurationProfile** from the **Configuration** dropdown list and then choose **AppConfigLabLinearTwentyPercentDeploymentStrategy** from the **Deployment strategy** dropdown list.

10. Enter **Negative testing JSON Schema Validator** in the **Deployment description** field.

11. Select **Start deployment** to being the deployment and observe the error message returned indicating the value for **boolEnableLimitResults** is of the wrong type in our **AppConfigLabConfigurationDocument.json** configuration document. ![](../images/appconfig-json-validator-failure.png)

12. Select **Cancel** then select **Update configuration profile** from the **Actions** dropdown list.

13. Expand **Validator 1** section and modify the *type* value for the **boolEnableLimitResults** property from *string* to *boolean*.

14. Select **Update configuration profile**.

14. Repeat the process indicated above to deploy the configuration profile and observe that the deployment succeeds this time.

AppConfig supports JSON Schema version 4.X for inline schema validation.
