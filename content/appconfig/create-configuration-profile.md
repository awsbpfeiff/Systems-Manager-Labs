---
weight: 6
title: "Create Configuration Profile"
---

#### Create an AppConfig Configuration Profile

Next, we will need to create a Configuration Profile which establishes a reference to where your configuration data is stored.  AppConfig currently supports storing configuration data as an S3 object, a Systems Manager document or a Systems Manager parameter.  Since we stored our configuration data in S3, our Configuration Profile will reference the path to the file we created and uploaded to S3.

Use the following procedure to create a configuration profile for storing your configuration data in S3.

1. Select the **Configuration profiles** tab.

2. Select **Create configuration profile** and enter **AppConfigLabS3ConfigurationProfile** in the **Name** field.![](../images/appconfig-config-profile-metadata.png?width=70pc)
 
3. Select Next. 

4. Select **Amazon S3 object** from the **Configuration source provider** options presented.

5. Enter the S3 Path of the empty JSON file you created and uploaded to S3 earlier in the **S3 object source** field (e.g., s3://appconfig-lab-firstname-lastname/AppConfigLabConfigurationDocument.json) and then select **Next**.
![](../images/appconfig-config-profile-source.png?width=70pc)

6. Leave the default option to create a **New service role** selected.

7. Select **Remove** in the **Validator 1** section.  We will not create a validator at this time.  We will revisit JSON Schema and AWS Lambda validators in later sections of the lab.
 
8. Select **Create configuration profile**.  
