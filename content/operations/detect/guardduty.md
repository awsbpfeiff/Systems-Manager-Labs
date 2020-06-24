---
weight: 1
title: "GuardDuty & Session Manager"
---

1. Upload IP Blacklist File

    - Click [here](https://s3.console.aws.amazon.com/s3/home?region=eu-west-1#) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **S3** in the *AWS Services search box*
        - Select **S3**
    - Click on the bucket that begins **lab-imageuploadbucket-**
    - Click **Upload** button
    - Download and modify IP Blacklist
        - Download [firehol_level1.netset](https://iplists.firehol.org/files/firehol_level1.netset)
        - **IMPORTANT:** Open the file and remove all the lines above 0.0.0.0/8 
        - Save the file
    - Click **Add files**
    - Locate *firehol_level1.netset* and upload
    - Click **Next**
    - Click **Next**
    - Click **Next**
    - Click **Upload**
    - Select the file and click **Copy path** from the popover on the right to use later     

1. Navigate to GuardDuty

    - Click [here](https://eu-west-1.console.aws.amazon.com/guardduty/home?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **GuardDuty** in the *AWS Services search box*
        - Select **GuardDuty**

1. Configure GuardDuty

    - Click **Get started** if it is not yet enabled
    - Click **Enable GuardDuty** if it is not yet enabled
    - Click **Lists** in the navigation menu
    - Click **Add a threat list**
    - Enter **LabList** for *List name*
    - Enter the Link you copied earlier for Location
    - Choose **Plaintext** for *Format*
    - Tick *I agree*
    - Click **Add List**
    - Select the checkbox under *Active*

    > **Note:** The console will inform you that is has "Successfully updated the list. It might take up to 5 or more minutes to take effect."

1. Navigate to Systems Manager Session Manager

    >Session Manager is a fully managed AWS Systems Manager capability that lets you manage your Amazon EC2 instances, on-premises instances, and virtual machines (VMs) through an interactive one-click browser-based shell or through the AWS CLI. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys. Session Manager also makes it easy to comply with corporate policies that require controlled access to instances, strict security practices, and fully auditable logs with instance access details, while still providing end users with simple one-click cross-platform access to your managed instances. 

    - Click [here](https://eu-west-1.console.aws.amazon.com/systems-manager/session-manager?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Systems Manager** in the *AWS Services search box*
        - Select **Systems Manager**
        - Select **Session Manager** from the navigation menu

1. Generate some findings using the IP Blacklist

    - Click **Start session**
    - Choose an instance by selecting a radio button
    - Click **Start session**
    - Interact with one of the bad IPs
        - I.e.  `ping 31.217.192.121` 
        - Double check that the IP you use is located in the IP Blacklist file

1. Navigate to GuardDuty

    - Click [here](https://eu-west-1.console.aws.amazon.com/guardduty/home?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **GuardDuty** in the *AWS Services search box*
        - Select **GuardDuty**

1. Review the findings
    
    - After a few minutes, some findings should be presented.
    - Click on the row where **UnauthorizedAccess:EC2/MaliciousIPCaller.Custom** is the finding and **Instance: i-...** is the Resource

Don't forget to terminate your Session.

> You will see information regarding unauthorized access to a disallowed IP address. You have enabled and configured continous threat detection using [Amazon GuardDuty](https://aws.amazon.com/guardduty/) to provide security **controls** that help you to **detect** potentially malicious activity.

![GuradDuty Findings](../../images/guardduty-findings.png)
