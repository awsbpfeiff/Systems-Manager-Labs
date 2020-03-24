# AWS Systems Manager Workshop Introduction

This workshop is intended to provide a 200-300 level hands on experience with core AWS Systems Manager capabilities. Lets start with a brief overview of AWS Systems Manager capabilities: 

1.	**Resource Groups** - Group AWS resources together by any purpose or activity you choose, such as application, environment, region, project, campaign, business unit, or software lifecycle.
2.	**Documents** - Centrally define the configuration options and policies for your managed instances.
3.	**Run** - Run a command, with rate and error controls, that targets an entire fleet of managed instances. 
4.	**Session Manager** - Securely connect to a managed instance with a single click, without having to open an inbound port or manage SSH keys. 
5.	**Distributor** - Distributor lets you package your own software - or find AWS-provided software packages - to install on AWS Systems Manager managed instances.
6.	**State Manager** - Use and create runbook-style SSM documents that define the actions to perform on your managed instances.
7.	**Patch Manager** – Simplify your operating system patching process for Windows and Linux.
8.	**Maintenance Window** - Automatically perform tasks in defined windows of time.
9.	**Compliance** - Quickly see which resources in your account are out of compliance and take corrective action from a centralized dashboard.
10.	**Parameter Store** - Separate your secrets and configuration data from your code by using parameters, with or without encryption, and then reference those parameters from a number of other AWS services.
11.	**Inventory** - Perform automated inventory by collecting metadata about your Amazon EC2 and on-premises managed instances. Metadata can include information about applications, network configurations, and more. 
12.	**Automation** - Automate or schedule a variety of maintenance and deployment tasks.
13.	**OpsCenter** - Centrally view, investigate, and resolve operational work items related to AWS resources.
14.	**Change Calendar** - Create calendar events to allow or block changes to your AWS resources
15.	**Explorer** - View consolidated inventory data from multiple AWS Regions and accounts that you manage. 
16.	**AppConfig** - Create, manage, and safely deploy application configuration data to your targets at runtime.
17.	**Hybrid Activations** - Create an activation to register on-premises servers and virtual machines (VMs), non-AWS Cloud servers, and other devices with AWS Systems Manager. Centrally manage Amazon EC2 instances and your hybrid environment from one location.


## Accessing AWS Account for Hands-on Labs

1.	Browse to https://dashboard.eventengine.run 
2.	Log in with the team hash that is provided to you – This will be your own account

![Event Engine first image](https://github.com/billpfeiffer/Systems-Manager-Labs/blob/master/images/event_engine_1.png)

3.	Log into the console using the provided Login Link

![Event Engine second image](https://github.com/billpfeiffer/Systems-Manager-Labs/blob/master/images/event_engine_2.png)
	 
Note: Once the event is complete, the account will be deleted.  

## Setup - Managed Instances

We will need to have a couple instances deployed to work with throughout the workshop.  This will be provided to the participants as homework ahead of the day or the workshop to ensure we have enough time to get through the material. 

## Create Key Pair
1.	In the management console navigate to us-east-1
2.	Use your administrator account to access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
3.	In the EC2 navigation pane under **Network & Security**, select Key Pairs and then select **Create Key Pair**.
4.	In the **Create Key Pair** dialog box, type a **Key pair name** such as **SM-Workshop** and then select **Create**.
5.	Save the keypairname.pem file for optional later use accessing the EC2 instances created in this lab.

## Create a Managed Instance IAM Role / Instance Profile
An IAM role is used to register instances with Systems Manager.  IAM role is used for Systems Manager Managed Instance (MI) in AWS and MI’s using an activation code in hybrid configurations. 

1. Create an Instance Profile for Systems Manager managed instances:
   - Navigate to the IAM console
   - In the navigation pane, select **Roles**.
   - Then select **Create role**.
   - In the **Select type of trusted entity** section, verify that the default **AWS service** is selected.
   - In the **Choose the service that will use this role** section, scroll past the first reference to EC2 **(EC2 Allows EC2 instances to call AWS services on your behalf)** and choose **EC2** from within the field of services. This will open the Select your use case section further down the page.
   - In the **Select your use case section**, choose **EC2 Role for AWS Systems Manager** to select it.
   - Select **Next: Permissions**.
2. Under **Attached permissions policy**, verify that **AmazonEC2RoleforSSM** is listed
   - Select **Next: Tags** – Do not add any
   - Select **Next: Review**
3. In the **Review** section:
   - Enter a **Role name**, such as **SM-Workshop-ManagedInstancesRole**.
     - This info will be used later on in the lab when provisioning new instances
   - Accept the default in the **Role description**.
   - Choose **Create role**.


