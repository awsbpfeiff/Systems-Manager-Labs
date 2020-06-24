---
weight: 4
title: "Launch Template"
---

### Launch Template

You can create a launch template that contains the configuration information to launch an instance. Launch templates enable you to store launch parameters so that you do not have to specify them every time you launch an instance. For example, a launch template can contain the AMI ID, instance type, and network settings that you typically use to launch instances. When you launch an instance using the Amazon EC2 console, an AWS SDK, or a command line tool, you can specify the launch template to use.

For each launch template, you can create one or more numbered launch template versions. Each version can have different launch parameters. When you launch an instance from a launch template, you can use any version of the launch template. If you do not specify a version, the default version is used. You can set any version of the launch template as the default version—by default, it's the first version of the launch template.

Launch templates are useful for defining instance configurations that you want to be used with licenses products managed in AWS License Manager.

Launch templates include the AMI to use when launching the instance, when instances are launched via the launch template, license usage and enforcement will be managed
through AWS License Manager.

####  Create a Launch Template From A Running Instance
We can create a launch template manually or we can jump start the definition by creating a launch template from a running instance.

For this lab, we will create a launch template from our running Windows Server 2016 with SQL Server instance.

1.  Open the [EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#Instances:sort=instanceId) listing your running instances.
2.  Check the **AWS License Manager - SQL Server 2016 Standard instance** and select **Actions->Create Template From Instance**
3.  For **Launch template name** enter W2016-with-SQL-Server-Standard
4.  For **Template version description** enter "Development configuration for SQL Server" 
5.  For AMI, scroll down to the **My AMIs** section and select the Windows AMI we created in setup.  To ensure the right AMI is being selected, open the EC2 Console and view the [AMI catalog](https://console.aws.amazon.com/ec2/v2/home#Images:sort=name). 
![](../images/windows_ami_ssm_created.png)
6.  Select **Create Launch Template**
7.  Proceed to the [Launch Templates](https://console.aws.amazon.com/ec2/v2/home?#LaunchTemplates:) menu listing the launch template you just created.
![](../images/ssm_launch_template_menu.png)
8.  Select the launch template you created and select **Actions->Launch Instance From Template**
9.  Leave the default options the same and select **Launch instance from template**
10. Proceed to the  [AWS License Manager menu](https://console.aws.amazon.com/license-manager/home) to observe that the license consumed count has now increased to capacity (8 of 8 consumed)
