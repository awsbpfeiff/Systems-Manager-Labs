---
weight: 3
title: "Setup"
---
In this section of the lab, we will demonstrate how to store the AppConfig configuration data in S3 as a JSON file type.  Later in the lab, we will use the configuration data to toggle on and off features within our application as well as provide some additional application configuration parameters.

#### Create AppConfig Configuration Document

Use the following procedure to create the configuration document.  A configuration document is a JSON formatted file containing the properties and values of the configurations you would like to use in your applications.

On your local machine, create a json file with the contents shown below and name the file **AppConfigLabConfigurationDocument.json**.
``` 
{
   "boolEnableLimitResults": false,
   "intResultLimit": 0
} 
```

#### Create S3 Bucket to Store Your AppConfig Configuration Document

We will create the S3 Bucket to store your AppConfig Configuration Document created above.  Note that the S3 bucket will be configured **with Versioning enabled** as this is a requirement when using S3 to store your AppConfig configuration data.

Use the following procedure to create an S3 Bucket and populate it with the JSON file that you created above.

1. Visit the [S3](https://s3.console.aws.amazon.com/s3) console and choose Create bucket.

2. Provide a name in the **Bucket name** field and select **US East (N. Virginia) us-east-1** for the **Region** field.  It is recommended that you name your S3 Bucket in the format *appconfig-lab-firstname-lastname* in order to ensure it is globally unique.

3. Leave all other default options selected and then select **Create bucket** to create your bucket and return to the S3 console **Buckets** list.

#### Enable Versioning and Upload Your Configuration Document to S3

1. Locate your S3 Bucket from the list and select it.

2. Select the **Properties** tab option and then select **Versioning**.  

3. Change the selection to **Enable versioning** and then select **Save**.

4. Select the **Overview** tab and then select **Upload**.

5. Select **Add files** and browse to the location where you saved your empty JSON file **AppConfigLabConfigurationDocument.json.** Select the file and choose **Open**.

6. Select **Next** and leave the default options selected on the **Set permissions** step of the upload wizard.

7. Select **Next** and leave the default Storage class option set to **Standard** on the **Set properties** step of the upload wizard.

8. Select **Next**, then **Upload** to complete the upload process and return to the bucket contents list.

9. Select your file and then select the **Copy path** option.  Paste the path locally and save it for later reference.
