---
weight: 7
title: "Cloudformation Support"
---

You can use AWS CloudFormation to specify license configuration rules within the templates. Deployments using these templates are tracked using License Manager. You can enforce rules on AWS CloudFormation templates by specifying one or more license configuration ARNs in the License Configurations field.

We are going to use SQL Server license configuration that we have setup initially for this walkthrough. 

**SQL Server 2016 Standard license configuration**

| **License Manager Rule**  | **Settings**                        |
| ------------------------- |:-----------------------------------:|
| License counting type     | **License Type** is set to **vCPU** |
| License count             | **Number of vCPUs** set to **8**    |
| Minimum / Maximum vCPUs   | Minimum vCPU **4** / Max vCPU**8**  |
| License count hard limit  | Enforce license limit = **Yes**     |
| Allowed tenancy           | **Tenancy** = **Default (shared)**  |

In order to configure AWS Cloudformation template to use the SQL Server 2016 Standard policy, we need to retrieve the License Configuration ARN. 

**To retrieve the License configuration ARN via console**

1. Open the [License Manager console](https://console.aws.amazon.com/license-manager/).
2. In the left navigation pane, choose **License configurations**.
3. Click on the **SQL Server 2016 Standard** license configuration.
4. Once in the policy detail page, click on the **Additional configuration details**. 
5. On the bottom-left side, you'll find the **License Configuration ARN**
![](../images/cnfconfig2.png)

You can also use the AWS CLI to retrieve a list of all the license configurations. In order to use this command, ensure you have setup the AWS CLI
appropriately and you have set the appropriate credentials for your account and have specified the region as well in **AWS_DEFAULT_REGION**.  
In the example below the output is redacted to save a space.

```
aws license-manager list-license-configurations
```

```
{
    "LicenseConfigurations": [
        {
            "LicenseConfigurationId": "lic-xxxxx",
            "LicenseConfigurationArn": "<<License_Configuration_Arn>>",
            "Name": "SQL Server 2016",
            "LicenseCountingType": "vCPU",
            "LicenseRules": [
                "#honorVcpuOptimization=true"
            ],
            "LicenseCount": 4,
            "LicenseCountHardLimit": false,
            "ConsumedLicenses": 0,
            "Status": "AVAILABLE",
```


With the license configuration ARN at hands, we are going to change one of the CloudFormation templates that we used in the begining of the workshop and specify the license configuration(s) that we want to use. 

You can download the CloudFormation stacks here: [Oracle](../templates/OraclewithRole.yml) and [SQL Server](../templates/SQLServerwithRole.yml).

The AWS CloudFormation [**EC2 Instance**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html) resource type allows you to specify a collection of [**License Specifications**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html#cfn-ec2-instance-licensespecifications) that you can use to enforce the license configuration desired.  

Below you can see how to configure AWS CloudFormation template to use a license configuration - in this example we are using the **SQL Server template** recently downloaded:
```
    Properties:
      ImageId: !FindInMap
        - RegionMap
        - !Ref 'AWS::Region'
        - WindowsAMI
      InstanceType: t3.xlarge
      Monitoring: 'true'
      DisableApiTermination: 'false'
      IamInstanceProfile:
        Ref: SSMInstanceProfile
      LicenseSpecifications:
      - LicenseConfigurationArn: <<Replace_with_License_Configuration_ARN>>
```

We've added a new property called **LicenseSpecifications** and within this property we set which license configuration we wanted to use for that specific Amazon EC2 instance.

Once you've saved your change on the CloudFormation template, you can create a new stack using it. 

1. Open the [CloudFormation](https://console.aws.amazon.com/cloudformation/) console
2. Click on Create Stack and select With new resources (standard) 
![](../images/cfnsql1.png)
3. In the Create stack page, select **Template is ready**, **Upload a template file**, and click **Next** after uploading the **SQLServerwithRole.yml** file which you downloaded earlier. 
![](../images/sqlservercfnupload.png)
4. Set "SQLServer2016" (no spaces) for the **Stack Name** and click **Next**. 
![](../images/cfnsql3.png)
5. Add the following for the Tags section and click **Next**
  * **Key** — Name
  * **Value** — SQL Server 2016 Standard
![](../images/cfnsql4.png)
6. On the review page, check the **I acknowledge that AWS CloudFormation might create IAM resources** and select **Create stack**.
![](../images/cfnsql5.png)
7.  If all the licenses are in use, the stack creation should fail with an error.  
![](../images/licenseexceededcfn.png)
If there enough licenses available to launch the instance, then you'll be able to see the updoted license count on the 
[**AWS Licenses Manager Dashboard**](https://console.aws.amazon.com/license-manager/home?region=us-west-2#/dashboard)