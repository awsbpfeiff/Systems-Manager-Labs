---
weight: 1
title: "Setup"
---

#### Provision POC Environment

Before starting, we will provision the proof of concept environment.  We will launch a couple of EC2 Instances, one with Microsoft SQL Server 2016 and another one with Oracle Database 18c Express Edition.
  
1.  Switch to the region that you wish to use.  Both us-east-1 and us-west-1 are supported.
2. Click on [Launch AWS License Manager resources](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=LicenceMgrLab&templateURL=https://s3.amazonaws.com/workshop.aws-management.tools/licensemanager/templates/licensemgr.yaml)
3. On the **Quick create stack** page, check the **I acknowledge that AWS CloudFormation might create IAM resources** and select **Create stack**.
![](../images/cfn5.png)

The Cloudformation stack creates the instances and installs the software used in the lab.

#### Permissions for the Core License Manager Role

License Manager uses the service-linked role named **AWSServiceRoleForAWSLicenseManagerRole**. This allows License Manager access to AWS resources to manage licenses on your behalf.

This role may already exist in your account if you have already setup License Manager.

The **AWSServiceRoleForAWSLicenseManagerRole** service-linked role trusts the license-manager.amazonaws.com service to assume the role.  A detailed list of policies required for the role can be found [here](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager-role.html).

You don't need to manually create a service-linked role. When you complete the License Manager first-run experience form the first time that you visit the License Manager console, the service-linked role is automatically created for you. 

**To get started creating the service-linked role, we will follow the instructions below:**

1. Open the [License Manager console](https://console.aws.amazon.com/license-manager/).
2. If prompted, click on **Start using AWS License Manager**, otherwise, AWS License Manager may have already been setup and you can proceed to the next section.
![](../images/StartwithLM.png)
3. When prompted, check the **I grant AWS License Manager the rquired permissions** checkbox and click on **Grant permissions**.  
![](../images/servicelinkedrole.png)

Optional: Click on View details to view required permissions.


#### Verify Systems Manager Setup an Inventory Collection

**NOTE:  It will take a few minutes after the Cloudformation stack is created before the instances and inventory is visible.  The Windows instance takes longer to become visible**

**To verify inventory is working as expected, we will follow the instructions below:**

1. Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.
2. On the left hand side navigation, we will click on **Managed Instances**.
3. You should see two instances whose name begins with "AWS License Manager", one for Windows and one for CentOS.  Click
on the instance ID for the Windows instance.
![](../images/ssm_managed_instances.png)
4. Then, click on the **Inventory** tab.  The **Inventory type: AWS:Application** lists the software discovered for the instance.
![](../images/ssm_inventory.png)

#### Create AMIs for License Manager

We will use AWS Systems Manager to create AMIs of our instances which will be used for license configurations in AWS License Manager.
In order to simplify this lab, we are using AWS license included AMIs for Windows Server 2016 with SQL Server 2016 Standard.  In practical
implementations, we may consider either importing an on premises VM with Windows Server & SQL Server or installing SQL Server on a Windows 
AMI with the purpose of using our existing purchased licenses.    

In order to associate an AMI with a license configuration, it has to exist in your account.  We are going to create AMI's from public AWS Marketplace
versions of our product.

**To get setup inventory with AWS Systems Manager Inventory, we will follow the instructions below:**

1. Open the [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/).
2. Click on **Automation** in the left hand navigation.
![](../images/ami1.png)
3. Click on **Execute Automation** on the right hand panel.
![](../images/ami8.png)
4. Under the **Document Categories** section, select **AMI management**.
5. Select the **AWS-CreateImage** Automation document and select **Next**.
![](../images/ami2.png)
6. In the **Execute automation document** page we will select **Rate control**.
![](../images/ami3.png)
7. Select **InstanceId** for the **Parameter** and **Parameter Values** for **Targets** in the **Targets** section.
![](../images/ami4.png)
8. In the **Input parameters** section, we will select the EC2 instances we launched earlier.  Set the **NoReboot** option to **0**.
![](../images/ssmcreateimageselectinstances.png)
9. For **Rate control**, set the value to 2 targets and set Error threshold to 1 errors and click **Execute**.
![](../images/create_image_rate_control.png)
10. The execution will take a few minutes to complete.
![](../images/ami7.png)
