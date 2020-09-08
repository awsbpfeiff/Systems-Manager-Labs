---
title: "Resource Groups"
date: 2020-06-T14:10:29-04:00
weight: 1
chapter: false
pre: "<b>3. </b>"
---

You can use *resource groups* to organize your AWS resources. Resource groups make it easier to manage, monitor, and automate tasks on large numbers of resources at one time.

AWS Resource Groups provides two general methods for defining a resource group. Both methods involve using a query to identify the members for a group.

The first method relies on tags applied to AWS resources to add resources to a group. Using this method, you apply the same tag key-value pairs to resources of various types in your account and then use the AWS Resource Groups service to create a group based on that tag pair or multiple pairs.

The second method is based on resources available in an individual AWS CloudFormation stack. Using this method, you choose an AWS CloudFormation stack, and then choose resource types in the stack that you want to be in the group.

You can specify a resource group as the target for the following Systems Manager capabilities:

* A command you run in Run Command.

* An automation workflow you run in Automation.

* An association you create in State Manager.

* A maintenance window you create in Maintenance Windows.

* A package installation or update operation in Distributor.

In this lab we will cover creating a Resource Group and some tags that will be used later on in the Patch Manager lab.  

### Getting Started

1. Open the AWS Resource Groups console at https://console.aws.amazon.com/resource-groups.
1. In the navigation pane, choose **Tag Editor** in the **Tagging** section, and perform the following steps.
    - For **Region** ensure ```us-east-1``` is selected.
    - For **Resource types**, choose ```AWS::EC2::Instance```.
    - Select **Search resources**.
    - Select the two instances with the **Tag: Name** of ```App1``` and ```App2``` from the four instances.
    - Select **Manage tags of selected resources** and perform the following steps.
        - Choose **Add tag**.
        - For **Tag key**, enter ```Patch Group```.
        - For **Tag value - *optional***, enter ```App```.
    - Select **Review and apply tag changes**.
1.  In the navigation pane, select **Create Resource Group** in the **Resources** section, and perform the following steps.
    - Select **Tag based**.
    - In the **Grouping criteria** section, choose **AWS::EC2::Instance** from the drop-down menu.
    - For **Tags**, enter the following:
        - For **Tag key**, enter ```Patch Group```.
        - For **Optional tag value**, enter ```App```.
        - Select **Add**.
    - Select **Preview group resources**.
    ![](./media/image3.png)
1.  For **Group name**, enter **App**.
1.  In the **Group tags - *optional*** section, you can optionally add tags to the resource group itself. For example, ```Key: App``` and ```Value: Front-end```.
    - **Important**: These group tags are not added to the individual resources.
1.  Select **Create group**.
1.  Repeat the above steps for the remaining two EC2 instances:
   - For **Group name**, enter **Web**
   - Use the following tags with the key-value pair of ```Patch Group``` : ```Web```.