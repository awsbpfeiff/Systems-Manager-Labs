---
title: "Lab Setup"
date: 2020-06-T14:10:29-04:00
weight: 1
chapter: false
pre: "<b>2. </b>"
---

## Setup - Managed Instances

#### Erik note -- These need to be provisioned during the event as EE accounts can only last for so long.

We will need to have a couple instances deployed to work with throughout the workshop.  This will be provided to the participants as homework ahead of the day or the workshop to ensure we have enough time to get through the material. 

### Create an EC2 Key Pair

[Amazon EC2 uses public-key cryptography](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) to encrypt and decrypt login information. Public-key cryptography uses a public key to encrypt a piece of data, such as a password, then the recipient uses the private key to decrypt the data. The public and private keys are known as a key pair. To [log in to the Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) we will create in this lab, you must create a key pair, specify the name of the key pair when you launch the instance, and provide the private key when you connect to the instance.

1. Use your administrator account to access the Amazon EC2 console at <https://console.aws.amazon.com/ec2/>.
1. In the EC2 navigation pane under **Network & Security**, choose **Key Pairs** and then choose **Create Key Pair**.
1. In the **Create Key Pair** dialog box, type a **Key pair name** such as `SM-Workshop`, choose **pem**, and then choose **Create key pair**.
1. **Save the** `keyPairName.pem` **file** for optional later use [accessing the EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) created in this lab.

### Create a Managed Instance IAM Role / Instance Profile
An IAM role is used to register instances with Systems Manager. IAM role is used for Systems Manager Managed Instance (MI) in AWS and MI’s using an activation code in hybrid configurations. 

1. Use your administrator account to access the Systems Manager console at <https://console.aws.amazon.com/systems-manager/>.
1. Choose **Managed Instances** from the navigation bar. If you have not satisfied the pre-requisites for Systems Manager, you will arrive at the **AWS Systems Manager Managed Instances** page.
   * As a user with AdministratorAccess permissions, you already have [User Access to Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-access-user.html).
   * The Amazon Linux AMIs used to create the instances in your environment have the [SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) installed by default.
   * If you are in a [supported region](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) the remaining step is to configure the IAM role for instances that will process commands.
1. Create an Instance Profile for Systems Manager managed instances:
   1. Navigate to the [IAM console](https://console.aws.amazon.com/iam/)
   1. In the navigation pane, choose **Roles**, and then choose **Create role**.
   1. In the **Select type of trusted entity** section, verify that the default **AWS service** is selected.
   1. In the **Choose a use case** section, select the first reference to EC2 (**EC2 Allows EC2 instances to call AWS services on your behalf**).
   1. Choose **Next: Permissions**.
   1. In the **Attach permissions policies** page, search for **AmazonSSMManagedInstanceCore**, select the check-box, and then choose **Next: Tags**.
   1. Optionally add tags for the IAM role and then choose **Next: Review**.
   1. In the **Review** section, enter a **Role name**, such as `ManagedInstancesRole`.
   1. Accept the default in the **Role description**.
   1. Choose **Create role**.
1. Apply this role to the instances you wish to manage with Systems Manager:
   1. Navigate to the [EC2 Console](https://console.aws.amazon.com/ec2/) and choose **Instances**.
   1. Select the first instance and then choose **Actions**, **Instance Settings**, and **Attach/Replace IAM Role**.
   1. Under **Attach/Replace IAM Role**, select **ManagedInstancesRole** from the drop down list and choose **Apply**.
   1. After you receive confirmation of success, choose **Close**.
   1. Repeat this process, assigning **ManagedInstancesRole** to each of the 3 remaining instances.
1. Return to the [Systems Manager console](https://console.aws.amazon.com/systems-manager/) and choose **Managed Instances** from the navigation bar. Periodically choose **Managed Instances** until your instances begin to appear in the list. Over the next couple of minutes your instances will populate into the list as managed instances. If the instance does not register after several minutes, you can reboot the EC2 instance by selecting **Actions**, **Instance State**, **Reboot**, within the EC2 console.

>**Note**<br> If desired, you can use a [more restrictive permission set](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-iam-user.html) to grant access to Systems Manager.

### Create IAM Role for Maintenance Window Tasks

This step is **critical** and could cause issues further into the workshop if not completed. 

https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-perm-console.html 

1. Create the role that allows Systems Manager to tasks in Maintenance Windows on your behalf:
   - Navigate to the **IAM console**.
   - In the navigation pane, select **Roles**, and then select **Create role**.
   - In the Select type of **trusted entity** section, verify that the default **AWS service** is selected.
   - In the **Choose the service that will use this role** section, choose **EC2**. This allows EC2 instances to call AWS services on your behalf.
   - Select **Next: Permissions**.
2. Under **Attached permissions policy**:
   - Search for **AmazonSSMMaintenanceWindowRole**.
   - Check the box next to **AmazonSSMMaintenanceWindowRole** in the list.
   - Select **Next: Tags**
   - Select **Next: Review**
3. In the **Review** section:
   - Enter a **Role name**, such as **SM-Workshop-MaintenanceWindowRole**.
   - Enter a **Role description**, such as Role for Systems Manager Maintenance Window.
   - Select **Create role**. Upon success you will be returned to the **Roles** screen.
4. To enable the service to run tasks on your behalf, we need to edit the trust relationship for this role:
   - Select the role you just created to enter its **Summary** page.
   - Select the **Trust relationships** tab.
   - Select **Edit trust relationship**.
   - Delete the current policy, and then copy and paste the following policy into the **Policy Document** field:
```
{
	"Version": "2012-10-17",
	"Statement": [{
		"Sid": "",
		"Effect": "Allow",
		"Principal": {
			"Service": [
				"ec2.amazonaws.com","ssm.amazonaws.com","sns.amazonaws.com"
			]
		},
		"Action": "sts:AssumeRole"
	}]
}
```
5. Select **Update Trust Policy**. You will be returned to the now updated Summary page for your role.
6. Copy the **Role ARN** to your clipboard by choosing the double document icon at the end of the ARN.

### Assign IAM PassRole Permissions
When you register a task with a Maintenance Window, you specify the role you created, which the service will assume when it runs tasks on your behalf. To register the task, you must assign the IAM PassRole policy to your IAM user account **(TeamRole)**. The policy in the following procedure provides the minimum permissions required to register tasks with a Maintenance Window.

1. To create the IAM PassRole policy for your Administrators IAM user group:
   - In the IAM console navigation pane, select Policies, and then select **Create policy**.
   - On the Create policy page, in the **Select a service area**, next to **Service** select **Choose a service**, and then select **IAM**.
   - In the **Actions** section, search for **PassRole** and check the box next to it when it appears in the list.
   - n the **Resources** section, select “You choose actions that require the role resource type.“, and then select **Add ARN** to restrict access. The Add ARN(s) window will open.
   - In the **Add ARN(s)** window, in the **Specify ARN for role** field, delete the existing entry, paste in the role ARN you created in the previous procedure, and then select **Add** to return to the Create policy window.
   - Select **Review policy**.
   - On the **Review Policy** page, type a name in the Name box, such as **SSM-Workshop-MaintenanceWindowPassRole-Policy** and then select **Create policy**. You will be returned to the **Policies** page.
2. To assign the IAM PassRole policy to your TeamRole:
   - In the IAM console navigation pane, select **Roles**, and then select your **TeamRole** to reach its Summary page.
   - Under the permissions tab, select **Attach Policy**.
   - On the **Attach Policy** page, search for **SSM-Workshop-MaintenanceWindowPassRole-Policy**, check the box next to it in the list, and select **Attach Policy**. You will be returned to the Summary page for the group.

### Deploy Managed Instances

In this section we will deploy 4 managed instances that we will work with throughout the lab.

1.  Navigate to the [EC2 Console](https://console.aws.amazon.com/ec2)
2.  Go to Instances
3.  Launch Instance
    - Amazon Linux 2 (64-bit)
    - T2.micro
    - Configure instance details
    - Number of instances: 4
    - Default VPC
    - No preference on subnet
    - Ensure auto-assign public IP is enabled
    - IAM Role: **SM-Workshop-ManagedInstancesRole** (previously
        created -- this is what allows instances to work with Systems
        Manager)
    - Default Storage
    - Leave Tags as is -- We will create some later in the workshop
    - Create a new Security Group -- Allow TCP 22 from anywhere
    - Launch
    - Select the Key Pair that you previously created

4.  Go back to view instances and ensure that all 4 transition to an Instance State of running

5.  Add tag
    - key:**Name** and value:**App1/App2** for two of the four
        instances
    - key:**Name** and value:**Web1/Web2** for remaining two
