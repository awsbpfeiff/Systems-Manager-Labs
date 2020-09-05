---
title: "Lab Setup"
date: 2020-06-T14:10:29-04:00
weight: 1
chapter: false
pre: "<b>2. </b>"
---

## Setup - Managed Instances

We will need to have a couple instances deployed to work with throughout the workshop.  This will be provided to the participants as homework ahead of the day or the workshop to ensure we have enough time to get through the material. 

## Create Key Pair
1. In the AWS management console navigate to us-east-1.
2. Use your administrator account to access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
3. In the EC2 navigation pane under **Network & Security**, select Key Pairs and then select **Create Key Pair**.
4. In the **Create Key Pair** dialog box, type a **Key pair name** such as **SM-Workshop** and then select **Create**.
5. Save the keypairname.pem file for optional later use accessing the EC2 instances created in this lab.

## Create an IAM Role and Instance Profile for Systems Manager
By default, AWS Systems Manager doesn't have permission to perform actions on your instances. You must grant access by using an AWS Identity and Access Management (IAM) instance profile. An instance profile is a container that passes IAM role information to an Amazon Elastic Compute Cloud (Amazon EC2) instance at launch. Servers and virtual machines (VMs) in a hybrid environment require an IAM role to communicate with the Systems Manager service. The role grants AssumeRole trust to the Systems Manager service. You only need to create the service role for a hybrid environment once for each AWS account.

1. Create an Instance Profile for Systems Manager managed instances:
   - Navigate to the IAM console
   - In the navigation pane, select **Roles**.
   - Then select **Create role**.
   - In the **Select type of trusted entity** section, verify that the default **AWS service** is selected.
   - Immediately under **Choose the service that will use this role**, choose **EC2**, and then choose **Next: Permissions**.
1. On the **Attach permissions policies** page, do the following:
   - Use the **Search** field to locate the **AmazonSSMManagedInstanceCore**. Select the box next to its name.
1. Choose **Next: Tags**.
1. Choose **Next: Review** as we are not adding any tags to the IAM instance role.
1. In the **Review** section:
   - Enter a **Role name**, such as **SM-Workshop-ManagedInstancesRole**.
     - This info will be used later on in the lab when provisioning new instances
   - Accept the default in the **Role description**.
   - Choose **Create role**.

## Deploy Managed Instances

In this section we will deploy four EC2 managed instances that we will work with throughout the workshop.

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2.
1. In the navigation pane, select **Instances**.
1. Choose **Launch Instance**.
    - On the **Step 1: Choose an Amazon Machine Image (AMI)** page, select **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
    - On the **Step 2: Choose an Instance Type** page, select **t2.micro**, and choose **Next: Configure Instance Details**.
    - On the **Step 3: Configure Instance Details** page, perform the following steps:
        - For **Number of instances**, enter ```4```.
        - For **Auto-assign Public IP**, ensure auto-assign public IP is enabled.
        - For **IAM Role**, select the IAM role previously created, **SM-Workshop-ManagedInstancesRole**. This is what allows instances to be managed by Systems Manager.
        - Choose **Next: Add Storage**.
    - Leave the defaults and choose **Next: Add Tags**.
        - Do not add tags as we will create some later in the workshop.
    - Choose **Next: Configure Security Group**.
    - On the **Step 6: Configure Security Group** page, choose **Select an existing security group** and then select the Security Group named **default** with the description **default VPC security group**.
    - Choose **Review and Launch**.
    - Choose **Launch**.
    - Choose **Proceed without a key pair** from the drop-down menu and select the box for **I acknowledge that I will not be able to connect to this instance unless I already know the password built into this AMI.**
        - We do not need to launch our EC2 instances with key pairs as we can remotely connect later in the workshop using Session Manager.
    - Choose **Launch Instances**.

1. Go back to view instances and ensure that all four EC2 instances transition to an **Instance State** of ```running```.
1. For two of the EC2 instances, add the following tags:
    - For **Key**, enter ```Name```.
    - For **Value**, enter ```App1``` for one instance and ```App2``` for the other.
    - Choose **Save**.
1. For the remaining two EC2 instances, add the following tags:
    - For **Key**, enter ```Name```.
    - For **Value**, enter ```Web1``` for one instance and ```Web2``` for the other.
    - Choose **Save**.