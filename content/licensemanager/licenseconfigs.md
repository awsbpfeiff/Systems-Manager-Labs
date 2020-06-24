---
weight: 3
title: "License Configurations"
---
## Licensing Configurations
License configurations are the core of License Manager. They contain licensing rules based on the terms of your enterprise agreements. The rules that you create determine how AWS processes commands that consume licenses. While creating license configurations, work closely with your organization's compliance team to review your enterprise agreements.

**Create a License Configuration** 
A license configuration represents the licensing terms in the agreement with your software vendor. Your license configuration specifies how your licenses should be counted (for example, by vCPUs or number of instances). It also specifies limits on your usage, so that you can prevent usage from going over the number of allocated licenses. Additionally, it can also specify other constraints on your licenses, such as the tenancy type.

**Optimize CPU Options**
Amazon EC2 instances support multithreading, which enables multiple threads to run concurrently on a single CPU core. Each thread is represented as a virtual CPU (vCPU) on the instance. An instance has a default number of CPU cores, which varies according to instance type. For example, an m5.xlarge instance type has two CPU cores and two threads per core by default—four vCPUs in total.

**Note:** Each vCPU is a thread of a CPU core, except for T2 instances.

In most cases, there is an Amazon EC2 instance type that has a combination of memory and number of vCPUs to suit your workloads. However, you can specify the following CPU options to optimize your instance for specific workloads or business needs:

* **Number of CPU cores:** You can customize the number of CPU cores for the instance. You might do this to potentially optimize the licensing costs of your software with an instance that has sufficient amounts of RAM for memory-intensive workloads but fewer CPU cores.
* **Threads per core:** You can disable multithreading by specifying a single thread per CPU core. You might do this for certain workloads, such as high performance computing (HPC) workloads.

You can specify these CPU options during instance launch. There is no additional or reduced charge for specifying CPU options. You're charged the same as instances that are launched with default CPU options. 

While creating a license configuration in AWS License Manager, you can define a **vCPU Optimization** license rule. With this rule set to True, License Manager counts vCPUs based on the customized core and thread count. Otherwise, License Manager counts the default number of vCPUs for the instance type. For more information about License Configuration Parameters and Rules click [here](https://docs.aws.amazon.com/license-manager/latest/userguide/config-overview.html)

Additional documentation on the Optimize CPU feature click [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-optimize-cpu.html#instance-cpu-options-rules)

**Limits**

* Number of license configurations per resource: 10
* Total number of license configurations: 25
* Systems Manager managed instances must be associated with vCPU and instance type license configurations

Complete list of License Configuration Parameters and Rules can be located [here](https://docs.aws.amazon.com/license-manager/latest/userguide/config-overview.html)

### Scenario #1 - SQL Server 2016 Standard
In this scenario, we will create a license configuration that will track SQL Server 2016 Standard licenses.

| **License Manager Rule**  | **Settings**                              |
| ------------------------- |:-----------------------------------------:|
| License counting type     | **License Type** is set to **vCPU**       |
| License count             | **Number of vCPUs** set to **8**          |
| Minimum / Maximum vCPUs   | Minimum vCPU **4** / Max vCPU**8**        |
| License count hard limit  | Enforce license limit = **Yes**           |
| Allowed tenancy           | **Tenancy** = **Default (shared)**        |
| Automatic Tracking        | SQL Server 2016 (track license included)  |


#### Create a license configuration using the console

1. Open the License Manager console at https://console.aws.amazon.com/license-manager/.
2. In the left navigation pane, choose **License configurations**.
![](../images/licconfig1.png)
3. Choose **Create license configuration**.
4. In the Configuration details panel, provide the following information:
    * **License configuration name** — SQL Server 2016 Standard
    * **Description** — SQL Server 2016 Standard
    * **License type** — vCPUs
    * **Number of vCPUs** — 8
    * **Enforce license limit** — Yes, the license limit is a hard limit.
![](../images/licconfig2.png)
5. Expand the Rules section and set the following:
    * **Minimum vCPUs** — 4
    * **Maximum vCPUs** — 8
    * **Tenancy** — Shared
![](../images/licconfig3.png)    
6.  AWS License Manager provides support for automatically tracking licenses for certain products installed on Systems Manager managed instances and automatically assigning them to the license configuration.  In Product Information, set the following:  
    * **Product Name** - SQL Server 
    * **Product Version** - 2016
    * **Don't track license included instances** - Unchecked
    For the purpose of this lab, we are going to leave the **Don't track license included instances unchecked**.  If we were using both AWS Marketplace license included
    instances as well as our own installations of SQL Server on EC2 managed instances, we may choose to only track licenses for the instances where we had installed SQL server
    ourselves and didn't have a license already included in the price.  
![](../images/licensemgrautomatic.png)  
7. (Optional) Expand the Tags pane to add one or more tags to your license configuration. Tags are key/value pairs. Provide the following information and then choose Add tag:
    * Key — The searchable name of the key - for example, enter **Vendor**.
    * Value — The value for the key - for example, enter enter **Microsoft**.
8. Choose **Submit** to create the license configuration. The console returns you to the License configurations page, which lists and describes your license configurations.

You may notice that the license configuration doesn't reflect our current running SQL Server instance yet.  AWS License Manager will periodically scan your AWS Systems Manager managed instances software inventory against
your license configurations and update the licenses consumed appropriately.   

#### Associate Policy to AMI
Once a license configuration has been created, it can be associated to existing resources for tracking or AMIs for license configuration enforcement.  
For the first scenario, we will associate the the policy we created to the AMI we created from our SQL Server 2016 Standard instance.

1. Open the License Manager console at https://console.aws.amazon.com/license-manager/.
2. In the left navigation pane, choose **License configurations**.
3. To create an AMI association, we will select the **SQL Server 2016 Standard** license configuration.
4. Go to **Actions** and select **Associate AMI**.  
![](../images/associateamisqlserver.png)
5. We will select the appropriate SQL Server 2016 AMI and click on **Associate**. 
To ensure the right AMI is being selected, open the EC2 Console and view the [AMI catalog](https://console.aws.amazon.com/ec2/v2/home#Images:sort=name).
Observe which AMI is Windows based, this is the AMI we will select.
![](../images/identifyamitypesqlserver.png)
![](../images/selectcreatedamisqlserver.png)
6. The AMI is now associated with the license configuration and you can see the associated ami on the configuration page, under the Associated AMIs tab.
![](../images/licenseassociatedwithsqlserverami.png)

#### Making sure the license configuration is working

To ensure our license configuration is working, we will attempt to launch a SQL Server 2016 Standard instance from the AMI we created and associated to our
licensing configuration.  This instance will attempt to exceed our licensing configuration, which means that the control plane should restrict this operation.

1. To create an EC2 instance we will go to the [EC2 Console](https://console.aws.amazon.com/ec2/) .
2. We will click on **Launch instance**.
![](../images/ec21.png)
3. Then, we will click on the **My AMIs** option and select the appropriate **Windows Server** AMI.
![](../images/launchwindowsami.png)
4. In the **Step 2: Choose an Insttance Type** page, select **t3.xlarge** as the instance type.  This instance has 4 vCPUs and will consume 4 of the 8 available vCPU licenses in
our license configuration.
5. Click on **Review and Launch**.
6. We will click on **Launch** on the **Step 7: Review Instance Launch** page.
![](../images/launchaminokey.png)
7. Now, if you check the license configuration in the [License Manager console](https://console.aws.amazon.com/license-manager/), it will reflect the licenses consumed by the instance you launched.
![](../images/sqlserverlicensesconsumed.png)

### Scenario #2 - Oracle Xe 18c
In this scenario, we will create a license configuration that will track Oracle Xe 18c licenses.  We are using the community edition for this lab.
In real scenarios, you would use a non-community version of the Database Management System (DBMS).  Click [here](https://www.oracle.com/assets/cloud-licensing-070579.pdf) to view Oracle licensing for cloud workloads.

| **License Manager Rule**  | **Settings**                        |
| ------------------------- |:-----------------------------------:|
| License counting type     | **License Type** is set to **vCPU** |
| License count             | **Number of vCPUs** set to **2**    |
| Minimum / Maximum vCPUs   | Minimum vCPU **4** / Max vCPU**2**  |
| License count hard limit  | Enforce license limit = **Yes**     |
| Allowed tenancy           | **Tenancy** = **Default (shared)**  |

#### Create a license configuration using the console

1. Open the License Manager console at https://console.aws.amazon.com/license-manager/.
2. In the left navigation pane, choose **License configurations**.
3. Choose Create license configuration.
![](../images/createlicenseconfig.png)
4. In the Configuration details panel, provide the following information:
    * **License configuration name** — Oracle Xe 18c
    * **Description** — Oracle Xe 18c
    * **License type** — vCPUs
    * **Number of vCPUs** — 2
    * Check **Enforce license limit** The license limit is a hard limit.
![](../images/licconfigora2.png)
5. Expand the Rules section and set the following:
    * **Minimum vCPUs** — 2
    * **Maximum vCPUs** — 2
    * **Tenancy** — Shared
6.  We will leave the Product information section empty.  
7. (Optional) Expand the Tags pane to add one or more tags to your license configuration. Tags are key/value pairs. Provide the following information and then choose Add tag:
    * Key — The searchable name of the key - for example, enter **Vendor**.
    * Value — The value for the key - for example, enter enter **Oracle**.
![](../images/licconfigora3.png)
8. Choose **Submit** to create the license configuration. The console returns you to the License configurations page, which lists and describes your license configurations.
![](../images/licconfigora4.png)

#### Associate configuration with discovered inventory item
Once a license configuration has been created, it can be associated to existing resources for license configuration enforcement.  
For this example, we will associate the the policy with the instance running Oracle Xe 18c.

1. Open the License Manager console at https://console.aws.amazon.com/license-manager/.
2. In the navigation pane, choose **Search Inventory**.
3. Use the **Application name** filter to scope the list of resources.  We will search for **Application name** Equals **oracle-database-18c**
![](../images/discoveroracle.png)
4. Once found, we will select the inventory item found (i.e. EC2 insntance) and click on **Associate license configuration**.
5. In the **Associate license configuration** window, we will select **Oracle Xe 18c** in the **License configuration name** drop down.
6. We will also select the **oracle-database-xe-18c** for product information.  Then, we will click on **Associate**.  This will automatically detect Systems Manager 
managed instances that are running this product and consume licenses for the product appropriately.
![](../images/associateoracle.png)
8. Click on  **License Configuration** link on the left hand navigation.
9. On the **License configurations** page, you will notice that all our existing licenses are being consumed.
![](../images/oraclelicensesconsumption.png)
