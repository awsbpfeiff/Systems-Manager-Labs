---
weight: 1
title: "Setup"
---

#### Lab Setup 
 
{{% notice info %}}
If your account has been provisioned for you by AWS for the purpose of completing this lab, you can skip the setup section as this infrastructure has been provisioned for you. Some of your resources will be preceded with mod-[ID]- instead of beginning with Lab.
&nbsp;  
&nbsp;  
You can access your account at the [dashboard login](https://dashboard.eventengine.run/login) and use the hash provided.
{{% /notice %}}

##### Lab Requirements 
 
You will need the following to be able to perform this lab: 
 
* The lab must be performed with a user that has administrator permissions
* An available VPC in the eu-west-1 region. 
 
##### Deploy the Lab Infrastructure using CloudFormation
 
There are two options for launching the CloudFormation Stack. The first option will automatically populate your CloudFormation stack variables. The second option will walk through all the steps involved in manually creating a new stack from a template URL. 
 
**Option A:** Automatically launch and populate stack creation variables 
 
[![Deploy Lab](../images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=eu-west-1&stackName=Lab&templateURL=https://workshop.aws-management.tools.s3.amazonaws.com/operations/templates/DistributedBase.yaml)
 
 
**Option B:** Manually create the stack 

1. **Copy the CloudFormation script URL** for this lab: 
{{< highlight text>}}
https://workshop.aws-management.tools.s3.amazonaws.com/operations/templates/DistributedBase.yaml
{{< /highlight >}}

1. Use your administrator account to access the [CloudFormation console](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template).

> **NOTE:** A CloudFormation template is a JSON or YAML formatted text file that describes your AWS infrastructure containing both [optional and required sections](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html). In the next steps, we will provide a name for our stack and parameters that will be passed into the template to help define the resources that will be implemented. 

##### Visualize in Designer

1. On the **Specify Template** step, next to Specify an Amazon S3 template URL, click the link to **View in Designer**.  
    - Take a moment to look at the graphical representation of the environment we are about to create, and the template in the JSON and YAML formats. 
    - The template was written in YAML as a nested stack and this is a method to convert it to JSON if that is your preference. 

    > **NOTE:** AWS CloudFormation Designer (Designer) is a graphic tool for creating, viewing, and modifying AWS CloudFormation templates. With Designer, you can diagram your template resources using a drag-and-drop interface, and then edit their details using the integrated JSON and YAML editor. Whether you are a new or an experienced AWS CloudFormation user, AWS CloudFormation Designer can help you quickly see the interrelationship between a template's resources and easily modify templates. 

1. Choose the <i class="fas fa-cloud-upload-alt"></i> **Create Stack** icon to return to the Specify Template step. 

    > **NOTE:** By clicking the Create Stack option in the Designer, you will automatically create a new cf-template S3 bucket in your selected region, and the original template will be copied with a unique random name and placed in your new S3 bucket.

##### Create Stack

1. On the *Specify Template* step, click the **Next** button. 
1. On the *Specify Details* step: 
    - Define your *Stack name*: by entering **Lab** 
    - Choose whether or not you want to complete the Systems Manager section of the lab
    - Provide your **email address**. This will only be used in the lab, and will only be used to send you SNS notifications of events. 
    - Select an *InstanceType*. We recommend selecting the default free tier eligible **t2.small** option. 

1. Click **Next** 
1. On the *On the Specify Details* step, under *Tags*:
    - Enter **Name** for *Key*
    - Enter **Lab** For *Value*
    - Click **Next** 
1. On the *Review* step: 
    - Review your selections 
    - Scroll down and tick **I acknowledge that AWS CloudFormation might create IAM resources with custom names.**. 
    - Scroll down and tick **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**
1. Click **Create stack** 
1. Navigate to the **Outputs** tab and make a note of the *ImagetrendsAppUrl* or open the link in a new tab.
 
**Confirm your subscription to the SNS Notification** 
    - After the CloudFormation script has completed, enter the email client for the address you provided earlier. 
    - Locate the email from AWS Notifications with the subject AWS Notifications - Subscription Confirmation. 
    - Click the Confirm subscription link to confirm your subscription. 
 
> **Note:** In this lab you will interact with the web based application you just created. To generate activity logs you will need to upload pictures to your application server. You can use your own images or download sample images to use here: [Sample Photos](https://workshop.aws-management.tools/operations/templates/sample-photos.zip) The sample image file is 12MB. If you plan to use the sample images start the download now so it can complete before you need them. 