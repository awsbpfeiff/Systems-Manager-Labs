---
weight: 4
title: "Dedicated Hosts"
---

Amazon EC2 Dedicated Hosts allow you to use your eligible software licenses from vendors such as Microsoft and Oracle on Amazon EC2, so that you get the flexibility and cost effectiveness of using your own licenses, but with the resiliency, simplicity and elasticity of the AWS Cloud. 
An Amazon EC2 Dedicated Host is a physical server fully dedicated for your use, so you can help address corporate compliance requirements.

Amazon EC2 Dedicated Host is also integrated with AWS License Manager, a service which helps you manage your software licenses, including Windows and SQL Server licenses. In License Manager, you can specify your licensing terms for governing license usage, as well as your Dedicated Host management preferences for host allocation and host capacity utilization. Once setup, AWS takes care of these administrative tasks on your behalf, so that you can seamlessly launch virtual machines (instances) on Dedicated Hosts just like you would launch an EC2 instance with AWS provided licenses. 

Use [AWS License Manager](https://console.aws.amazon.com/license-manager/home) to start managing your dedicated hosts.

#### Save Money on Licensing Costs

Dedicated Hosts allow you to use your existing per-socket, per-core, or per-VM software licenses, including Microsoft Windows Server, Microsoft SQL Server, [SUSE Linux Enterprise Server](https://www.suse.com/solutions/public-cloud/), [Red Hat Enterprise Linux](http://www.redhat.com/en/technologies/cloud-computing/cloud-access), or other software licenses that are bound to VMs, sockets, or physical cores, subject to your license terms. This helps you to save money by leveraging your existing investments. Learn more about your [Windows licensing options](https://aws.amazon.com/windows/licensing/).

#### Help Meet Corporate Compliance Requirements

Some organizations need to run their instances on dedicated servers instead of multi-tenant servers. With Dedicated Hosts, you get a physical server that is dedicated for your use. Dedicated Hosts provide visibility and the option to control how you place your instances on a specific, physical server. This enables you to deploy instances using configurations that help address corporate compliance and regulatory requirements.

A common scenario are customers that choose to license their SQL Server licenses against the sockets or cores on a physical machine,they need to use these licenses on Dedicated Hosts. 
The use of Dedicated Hosts allows you use a per-core or per-socket SQL Server licensing model, and you do not need to have active Software Assurance for licenses that are purchased prior to October 1, 2019 and which are not upgraded to versions released after October 1, 2019.

Customers that want to have license usage reporting, can use [AWS License Manager](https://aws.amazon.com/license-manager/) to define license rules that allow them to ensure license usage per Sockets, Cores, vCPUs and Instances tenacy.

### Host Resource Groups
Host resource groups allow you to create a collection of EC2 dedicated hosts.  Host Resource Groups automate the management process of allocating and releasing hosts to match your runtime needs.

#### Create A Core Based License Configuration
We will create a license configuration based on cores that will be used with our host resource group.

1.  Open the [AWS License Manager License Configration](https://console.aws.amazon.com/license-manager/home) menu.
2.  Select **Create License Configuration**.
3.  For the configuration details, enter the following:
    * **License configuration name** — Oracle Per Core Licensing
    * **Description** — Oracle Per Core Licensing
    * **License type** — Cores
    * **Number of Cores** — 96
    * **Enforce license limit** — Checked, the license limit is a hard limit.
4.  Leave Product Information section blank and select **Submit**.

#### Associate License Configuration with AMI
We will associate the per core license configuration with the Oracle AMI that we created earlier.  
This AMI has Oracle community edition installed for the purposes of our workshop.  In a practical deployment you may install
a licensed version of Oracle for which you have per core license purchases that you want to use.

1.  To ensure the right AMI is being selected, open the EC2 Console and view the [AMI catalog](https://console.aws.amazon.com/ec2/v2/home#Images:sort=name).
Observe which AMI is Linux based based, this is the AMI we will select.
2.  Open the [AWS License Manager License Configration](https://console.aws.amazon.com/license-manager/home) menu.
3.  Select the **Oracle Per Core License** configuration we created in the previous section and select **Actions->Associate AMI**.
![](../images/associateoracleami.png)
4.  Select the Linux AMI that we noted is step 1.
![](../images/associatelinuxami_oracle.png)

#### Create Host Resource Group
We will now create our Host Resource Group and associate it to the license configuration we created in the previous section.  

1.  Open the [AWS License Manager](https://console.aws.amazon.com/license-manager) menu and select **Host resource groups** from the menu. 
2.  Select **Create host resource group** and enter the following details:
     * **Host resource group name** — Oracle-Per-Core-Licensing
     * **Description** — Oracle Per Core Licensing Resource Group
     * **EC2 Dedicated Host management settings**
       * Allocate hosts automatically (checked)
       * Release hosts automatically (checked)
       * Recover hosts automatically (checked) 
3.  For **Associated license configurations** select the licensing configuration we created earlier, **Oracle Per Core Licensing**
![](../images/associatepercorelicensing.png)
5. (Optional) Expand the Tags pane to add one or more tags to your license configuration. Tags are key/value pairs. Provide the following information and then choose Add tag:
    * Key — The searchable name of the key - for example, enter **Vendor**.
    * Value — The value for the key - for example, enter enter **Oracle**.
6.  Select **Create**


#### Launch a dedicated instance in instance in Host Resource Group
We'll now launch an EC2 instance using the Host Resource Group configuration we just created.


1. To create an EC2 instance we will go to the [EC2 Console](https://console.aws.amazon.com/ec2/) and select **Launch instance**.
![](../images/ec21.png)
2.  Select **My AMIs** and then select the Linux AMI we created in setup.
![](../images/launchoracleami.png)
3.  For **Instance Type** select **r5.large** and then select **Next: Configure Instance Details**.
![](../images/selectpercoreinstance.png)
4.  Confirm **Tenancy** is Dedicated Host and for Host resource group, check the **Launch instance into a host resource group** option and set it to the Host resource group we created earlier, Oracle-Per-Core_licensing.
![](../images/dedicated_host_launch.png)
5.  Select **Review & Launch** and launch the instance, you can launch the instance without a keypair for our purposes.
![](../images/launchaminokey.png)
6.  Proceed to the [Dedicated Hosts EC2 menu](https://console.aws.amazon.com/ec2/v2/home?#Hosts) to see the newly allocated dedicated host.
7.  Proceed to the [AWS License Manager License Configration](https://console.aws.amazon.com/license-manager/home) menu to see the newly consumed cores from the dedicated host.
8.  Terminate the instance from the [EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#Instances:sort=instanceType) to automatically release the dedicated host.  Check the instance that we launched and select **Actions->Instance State=>Terminate**.  
![](../images/terminateec2instance.png)
Wait until the instance state shows **terminated**
9.  Proceed to the [Dedicated Hosts EC2 menu](https://console.aws.amazon.com/ec2/v2/home?#Hosts:) to see the dedicated host has been automatically released. 
It may take some time for this to occur after the instance is terminated so you can proceed and come back to this step later.
