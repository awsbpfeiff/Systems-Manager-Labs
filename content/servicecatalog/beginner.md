---
weight: 3
title: "Beginner Labs"
---


### Beginner Labs

- [Service Actions](beginner/serviceaction)  
- [Deploy a Product](beginner/deploy)  
#### Pre-requisites:

1.  Create a user called **labadmin**, give it an an Administrator
    policy (optional if you have a user with an admin policy)

2.  Login to your AWS Account using the **labadmin** user (or an admin
    user)

3.  Use the **US East (N. Virginia)** region

#### Steps

Use the Launch Stack button to create

-   Service Catalog Portfolio

-   Service Catalog Products

-   Create Service Catalog components using - Sample CloudFormation

-   On the Create stack page, choose **next**

-   On the stack details page, fill in the parameters and then choose
    next

-   use the defaults for the stack Parameter

-   On the **Configure stack** options page choose **Next**

-   On **Review page**, choose the check box for I acknowledge that AWS
    CloudFormation might create IAM resources with custom names.

> Choose **Create Stack**

-   Wait for the status to chage to **CREATE\_COMPLETE**

> ![](../images/image3.png)

-   Select the Stack Name **SCloftLab**

-   Select the **Outputs** tab

> ![](../images/image4.png)

-   Copy the ouput section

-   Paste in the box below

#### Deploy a Service Catalog Product - Windows Server 

> ![](../images/image5.png)

1.  Select the **SwitchRoleSCEndUserRole** URL and open it in a new tab

2.  Choose **Switch Roles**

3.  Open the [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home) console

    > ![](../images/image6.png)

4.  Choose **Products list** and the three vertical dots next to
    **LABEC2**

5.  Choose **Launch product**

6.  Enter **EC200windows** as a provision product name

7.  Choose **Next**

8.  Enter the Parameters

    -   KeyName - pick one

    -   VpcId pick one

    -   Size - **t2.medium**

    -   ServerName - **xxlabWinServer**

    -   Imageid - **Use default**

    -   SubnetID pick one

9.  Choose **NEXT**

10. On the **Tag options** page choose **Next**

11. On the **Notifications** page choose **NEXT**

12. On the **Review** page choose **LAUNCH** This will deploy the
    Windows Server.

    > ![/Users/nickil/Desktop/AWS Immersion
    > Day-Bootcamp\_files/sclab006.png](../images/image7.png)

13. Wait for the **Status** to be **Succeeded** you can refresh if you
    need to

    > **Congratulations you now have a Windows Server**

14. View your server from the [EC2
    Console](https://console.aws.amazon.com/ec2/v2/home) Look for the
    server with the \'xxlabWinServer\' Name

    > ![](../images/image8.png)

### 

### 

### Find an Image Id for a Linux Server

1.  Choose the **Launch Instance** button

    > ![](../images/image9.png)

2.  Copy the **ami** from the first **Amazon Linux 2 AMI**

3.  Paste it here

4.  Return to [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home)

5.  Choose **Service Catalog** From the list of services

    > ![](../images/image6.png)

6.  Choose **Products list** and the three doot next to **LABEC2**

7.  Choose **Launch product**

8.  Enter **EC2001Linux** as a provision product name

9.  Choose **Next**

10. Enter the Parameters

    -   KeyName - pick one

    -   VpcId pick one

    -   Size - **t2.medium**

    -   ServerName - **xxlabLinuxServer**

    -   Imageid - **Use the AMI you just copied**

    -   SubnetID pick one

11. Choose **NEXT**

12. On the **Tag options** page choose **Next**

13. On the **Notifications** page choose **NEXT**

14. On the **Review** page choose **LAUNCH** This will deploy the
    Windows Server.

    > ![](../images/image7.png)

15. Wait for the **Status** to be **Succeeded** you can refresh if you
    need to

    > **Congratulations you now have a Linux Server**

16. View your server from the [EC2
    Console](https://console.aws.amazon.com/ec2/v2/home) Look for the
    server with the \'xxlabLinuxServer\' Name

    > ![](../images/image10.png)
### Give the user the ability to restart the server from Service Catalog (Service Actions)

1.  Switch Role to the **SwitchRoleSCAdmin** role

2.  Copy the url for **SwitchRoleSCAdmin** and use it in a new browser
    tab

    > you should still have the url in a text box on this page if you scroll
    > up ![](../images/image11.png)

3.  Return to [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home)

    > ![](../images/image12.png)

4.  Choose **Service actions**

    > ![](../images/image13.png)

5.  Choose **Create new action** button

6.  Choose **AWS-RestartEC2Instance**

7.  Choose **Next**

8.  On the **Configure** page use defaults , choose **Create action**

9.  Choose the **AWS-RestartEC2Instance** choose **Associate action**

    > ![](../images/image14.png)

10. Choose the product **LABEC2** choose **V1** version, choose
    **Associate action**

    > Great !! users can now restart their servers by themselves

### End-user Restart a server 

1.  Switch back to the End-user Role

    > ![](../images/image15.png)

2.  Choose the role name, choose **ServiceCatalogEndUser**

3.  Choose **Switch Role**

4.  Return to [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home)

5.  Choose **Provisioned product list**

6.  Choose a **Provisioned Product name**

    > ![](../images/image16.png)

7.  Choose the **ACTIONS** button, Choose **AWS-RestartEC2Instance**

8.  Choose **RUN ACTION**

    > ![](../images/image17.png)

9.  Switch to the [EC2
    Console](https://console.aws.amazon.com/ec2/v2/home) Look for the
    server restarting

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 