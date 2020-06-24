---
weight: 1
title: "Setup"
---

#### Lab Setup 
 
**Lab Requirements** 
 
You will need the following to be able to perform this lab: 
 
* The lab must be performed logged into a user with Administrator permissions and an existing EC2 Key Pair 
* An available VPC in the region us-east-1 region. 
 
**1.1 Deploy the Lab Infrastructure using CloudFormation** 
 
There are two options for launching the CloudFormation Stack. The first option will automatically populate your CloudFormation stack variables. The second option will walk through all the steps involved in manually creating a new stack from a template URL. 
 
**Option A:** Automatically launch and populate stack creation variables 
 
[**Deploy Lab by clicking here**](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=us-east-1&stackName=CWLab&templateURL=https://s3.amazonaws.com/workshop.aws-management.tools/cloudwatch/templates/distributed.yaml) 
 
**NOTE:** The automatic launch process will only work if you are using the new version of the CloudFormation console. 
 
If the button fails to take you to the create new stack page, try switching to the new CloudFormation console by clicking the New Console link in the CloudFormation drop-down menu. 
 
**Option B:** Manually walk through the stack creation from a template URL 
 
1. **Copy the CloudFormation script URL** for this lab: [distributed.yaml](https://s3.amazonaws.com/workshop.aws-management.tools/cloudwatch/templates/distributed.yaml)

**NOTE:** The instructions below are based on the new version of the CloudFormation console.If the button fails to take you to the create new stack page, try switching to the new CloudFormation console by clicking the New Console link in the CloudFormation drop-down menu. 

2. Use your administrator account to access the CloudFormation console at [https://console.aws.amazon.com/cloudformation/](https://console.aws.amazon.com/cloudformation/). 

3. Click the **Create Stack** button. 
    - On the **Specify Template** screen: 
        - Ensure the **Template is ready** radio button is selected under the Prerequisite - Prepare template section. 
        - Ensure that the **Amazon S3 URL** radio button is selected under the Specify template section and then paste the copied URL into the Amazon S3 URL textbox. 

**NOTE:** A CloudFormation template is a JSON or YAML formatted text file that describes your AWS infrastructure containing both [optional and required sections](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html). In the next steps, we will provide a name for our stack and parameters that will be passed into the template to help define the resources that will be implemented. 

4. On the **Specify Template** step, next to Specify an Amazon S3 template URL, click the link to View in Designer. 
    - Take a moment to look at the graphical representation of the environment we are about to create, and the template in the JSON and YAML formats. 
    - The template was written in YAML as a nested stack and this is a method to convert it to JSON if that is your preference. 
**NOTE:** AWS CloudFormation Designer is a graphic tool for creating, viewing, and modifying AWS CloudFormation templates. With Designer, you can diagram your template resources using a drag-and-drop interface, and then edit their details using the integrated JSON and YAML editor. AWS CloudFormation Designer can help you quickly see the interrelationship between a template’s resources and easily modify templates. 
5. Choose the Create Stack icon (a cloud with an arrow) to return to the Specify Template step. 
**NOTE:** By clicking the Create Stack option in the Designer, you will automatically create a new cf-template S3 bucket in your selected region, and the original template will be copied with a unique random name and placed in your new S3 bucket. 
6. On the Specify Template step, click the Next button. 
7. On the Specify Details page: 
    - Define your Stack name: by entering CWLab 
    - Provide your email address. This will only be used in the lab, and will only be used to send you SNS notifications of events. 
    - Define the HTTPLocation CIDR subnet address to limit who can connect to your resources. 
        - You can use [checkip.amazonaws.com/](http://checkip.amazonaws.com/) to identify your IP address and restrict access to it specifically by entering the IP followed by the CIDR subnet mask of /32 (with no spaces between IP and the CIDR subnet mask). 
    - Select an InstanceType. We recommend selecting the default free tier eligible t2.micro option. 
    - Select your EC2 KeyPair from the KeyName pull down list. 
    - Define your SSHLocation entering your IP address with CIDR subnet mask 
    - Define your Workload name as ExampleCorp 
8. Choose Next 
9. On the Options page optionally specify additional Tags, and then scroll down and choose Next 
10. On the Review page: 
    - Review your selections 
    - Scroll down and check the box I acknowledge that AWS CloudFormation might create IAM resources. 
11. Choose Create 
 
**1.2 Confirm your subscription to the SNS Notification** 
    - After the CloudFormation script has completed, enter the email client for the address you provided earlier. 
    - Locate and email from AWS Notifications with the subject AWS Notifications - Subscription Confirmation. 
    - Click through the Confirm subscription link to confirm your subscription. 
 
**Note:** In this lab you will interact with the web based application you just created. To generate activity logs you will need to upload pictures to your application server. You can use your own images or download sample images to use here: [Sample Photos](https://workshop.aws-management.tools/cloudwatch/templates/sample-photos.zip) The sample image file is 76MB. If you plan to use the sample images start the download now so it can complete before you need them. 
 
