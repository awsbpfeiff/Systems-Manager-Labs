---
weight: 16
title: "Best Practices"
---

AWS AppConfig uses the value of the ClientConfigurationVersion parameter to identify the configuration version on your clients. If you don’t send ClientConfigurationVersion with each call to GetConfiguration, your clients receive the current configuration. You are charged each time your clients receive a configuration.

To avoid excess charges, we recommend that you include the ClientConfigurationVersion value with every call to GetConfiguration. This value must be saved on your client. Subsequent calls to GetConfiguration must pass this value by using the ClientConfigurationVersion parameter. 

Sending ConfigurationVersion during subsequent polling for configuration updates is similar to the concept of [HTTP ETags](https://en.wikipedia.org/wiki/HTTP_ETag). 

Additionally, the ClientId parameter provided in the GetConfiguration API call should be a unique, user-specified ID to identify the client for the configuration. This ID enables AppConfig to deploy the configuration in intervals, as defined in the deployment strategy.


In the following exercise, we will demonstrate how to:

- Retrieve ConfigurationVersion value upon the first call to the GetConfiguration API and use it to set the value of ClientConfigurationVersion parameter for use in subsequent GetConfiguration API calls.
- Set the ClientId to a unique identifier 


Use the following procedure to set the ClientConfigurationVersion and ClientId parameters and cache them for future GetConfiguration API calls.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the functions list.

2. Select the **Configuration** tab and scroll down to the **Function code** section.

3. Locate the following line of code: `// add create UUID function code here`.

4. Replace this entire line with the code below.  We will use this `create_UUID` function to generate a unique *ClientId*.
    ```
    function create_UUID(){
        let dt = new Date().getTime();
        const uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
            var r = (dt + Math.random()*16)%16 | 0;
            dt = Math.floor(dt/16);
            return (c=='x' ? r :(r&0x3|0x8)).toString(16);
        });
        return uuid;
    }
    ```
5. Locate the following section of code and **delete** it.
    ```
          const params = {
            Application: 'AppConfigLab', /* required */
            ClientId: 'AppConfigLabAPIGatewayLambda', /* required */
            Configuration: 'AppConfigLabS3ConfigurationProfile', /* required */
            Environment: 'AppConfigLabLambdaDevelopment', /* required */
          };
          
          const appConfigResponse = await appconfig.getConfiguration(params).promise();
          const configData = Buffer.from(appConfigResponse.Content, 'base64').toString('ascii');
    ```
6. Locate the following line of code: `// add params and cache constants code here`. 

7. Replace this entire line with the code below. 
    ```
    const constParams = {
      Application: 'AppConfigLab', /* required */
      Configuration: 'AppConfigLabS3ConfigurationProfile', /* required */
      Environment: 'AppConfigLabLambdaDevelopment', /* required */
    };
    
    let cachedParams = {};
    let cachedConfigData = {};
    ```
    Note that the `constParams`, `cachedParams` and `cachedConfigData` constants are declared outside the body of the handler.  This is so that we can take advantage of the [AWS Lambda execution context](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html) to cache the GetConfiguration API parameters and the response we receive from the GetConfiguration API call for subsequent Lambda executions.

    The `constParams` constant contains the GetConfiguration API parameters that will not change with subsequent API calls.  The `cachedParams` variable is a placeholder to store ClientConfigurationVersion and ClientId.  The `cachedConfigData` variable is a placeholder to store our AppConfig configuration data in a *cache* within the Lambda execution context.

6. Locate the following line of code: `// add best practices client configuration code here`.

7. Replace this entire line with the code below.
    ```
      /* 
      checks if ClientId is present in cachedParams and, if not, 
      adds the ClientId property to the cachedParams object
      using the value returned for the create_UUID function call
      */
      if (!cachedParams.ClientId) {
        cachedParams.ClientId = create_UUID();
        console.log('ClientId: ', cachedParams.ClientId);
      }
      
      /* 
      merges constParams and cachedParams into 
      a new params constant using spread operator
      and outputs params to the console 
      */
      
      const params = {...constParams, ...cachedParams};
      console.log('params: ', params);
          
      /* 
      calls GetConfiguration API and outputs response to the console  
      */
      const appConfigResponse = await appconfig.getConfiguration(params).promise();
      console.log('appConfigResponse: ', appConfigResponse);
      
      /* 
      checks if ClientConfigurationVersion is present in cachedParams and, if not, 
      adds the ClientConfigurationVersion property to cachedParams
      setting its value equal to the ConfigurationVersion property of the AppConfig GetConfiguration API response
      */
      if ( (!cachedParams.ClientConfigurationVersion) || appConfigResponse.ConfigurationVersion !== cachedParams.ClientConfigurationVersion) {
        cachedParams.ClientConfigurationVersion = appConfigResponse.ConfigurationVersion;
        console.log('cachedParams: ', cachedParams);
      }
      
      console.log('appConfigResponse.Content: ', appConfigResponse.Content);
      
      const configData = await Buffer.from(appConfigResponse.Content, 'base64').toString('ascii');
      console.log('configData: ', configData);
      
      
      if ((!cachedConfigData) || (configData && cachedConfigData !== configData))
        cachedConfigData = configData;
        
      console.log('cachedConfigData: ', cachedConfigData);
    ```
8. Select **Save**.


#### Test the API Gateway Lambda and Observe the Behavior 

Next, we will test the Lambda from within the Lambda console to observe the behavior with these new parameters.

1. Visit the [Lambda](https://console.aws.amazon.com/lambda/) Console and select **AppConfigLabAPIGatewayLambda** from the functions list.

2. Select the **Configuration** tab and scroll down to the **Function code** section.

3. Select **Test**.  

4. When prompted to **Configure test event**, select **Create new test event** option and select **hello-world** from the **Event template** option.
 
5. Enter **AppConfigTestEvent** in the **Event name** field.

6. Replace the test event JSON body with an empty JSON object `{}`.  We won't need to pass anything into our test event for the purposes of this demonstration.

7. Select **Create** to save the test event and execute the Lambda.

8. Scroll to the top of the page and locate the **Execution result** section which is highlighted in green upon successful execution of your code.

9. Within **Execution result** section, locate the **Log output**.

Note that in the `params` output we now have a ClientId that is being passed into the GetConfiguration API call.  The ClientId was generated by the `create_UUID()` function we added early in this seciton of the lab.

    {
      Application: 'AppConfigLab',
      Configuration: 'AppConfigLabS3ConfigurationProfile',
      Environment: 'AppConfigLabLambdaDevelopment',
      ClientId: 'faa5c00d-8d84-4933-9f02-4bd33584c8c7'
    }

Next, locate the `appConfigResponse` in the output and observe the `ConfigurationVersion` property.  This value is returned within the AppCongig response and we will use it in subsequent calls to the GetConfiguration API.

    {
      ConfigurationVersion: 'dlKqxW4Qqy88kh6cBtZ9qGtPlshUYvH4',
      ContentType: 'application/octet-stream',
      Content: <Buffer 7b 0a 20 20 22 62 6f ... 9 more bytes>
    }


Also, note that in the `appConfigResponse` output the `<Buffer 7b 0a .... more bytes` value which is essentially the encoded configuration data.  When we test the Lambda again in a moment, you will notice that this will be empty.

#### Test the API Gateway Lambda a Second Time

Use the following procedure to test the Lambda again to observe the response from the GetConfiguration API call now that we are passing **ClientConfigurationVersion** as a parameter.

1. Select **Test** again and then scroll to the **Execution result** section to observe the **Log output**.

2. Take note of the differences in the `params` and `appConfigResponse` values from this second test execution.

From our **first** test execution the `params` value resembled this:

    {
      Application: 'AppConfigLab',
      Configuration: 'AppConfigLabS3ConfigurationProfile',
      Environment: 'AppConfigLabLambdaDevelopment',
      ClientId: 'faa5c00d-8d84-4933-9f02-4bd33584c8c7'
    }

As a result of our **second** execution, the `params` value resembled this:

    {
      Application: 'AppConfigLab',
      Configuration: 'AppConfigLabS3ConfigurationProfile',
      Environment: 'AppConfigLabLambdaDevelopment',
      ClientId: 'e7f0d3ca-162f-4bea-af70-a4f129ac4866',
      ClientConfigurationVersion: 'dlKqxW4Qqy88kh6cBtZ9qGtPlshUYvH4'
    }

The significant difference is that in the second execution we passed in **ClientConfigurationVersion**.  

Similarly, take a look at our value of `appConfigResponse` from our **first** execution.  Note it has `<Buffer 7b 0a .... more bytes` - basically, a long string of encoded data.

    {
      ConfigurationVersion: 'dlKqxW4Qqy88kh6cBtZ9qGtPlshUYvH4',
      ContentType: 'application/octet-stream',
      Content: <Buffer 7b 0a 20 20 22 62 6f 6f ... 9 more bytes>
    }
    
After the **second** execution, observe that the `<Buffer ` is empty.  Again, this is because AppConfig uses a concept similar to concept of [HTTP ETags](https://en.wikipedia.org/wiki/HTTP_ETag) in that it will only send updates in subsequent calls for a given ClientId and ConfigurationVersion.

    {
      ConfigurationVersion: 'dlKqxW4Qqy88kh6cBtZ9qGtPlshUYvH4',
      ContentType: 'application/json',
      Content: <Buffer >
    }

