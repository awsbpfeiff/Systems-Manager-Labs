---
weight: 2
title: "Systems Manager Inventory"
---

1. Navigate to Systems Manager
    - Click [here](https://eu-west-1.console.aws.amazon.com/systems-manager/home?region=eu-west-1#) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Systems Manager** in the *AWS Services search box*
        - Select **Systems Manager**


1. Inventory

    - Select **Managed Instances** from the navigation menu
    - Select **Setup Inventory** from the *Configure Inventory* dropdown
    - Enter **Inventory-Lab** as *Name*
    - Specify targets by selecting **Specifying a tag**
    - Enter **Name** for *tag key*
    - Enter **Lab App host** for *tag Value*
    - Click **Setup Inventory**
    - Click **View details** at the top of the page
    - **This may take a few minutes to complete**

    > You will know when the inventory is complete, because the *Top 5 Applications* will contain information. If the inventory has not completed, it will state that *There is no data to display*. You can always come back to complete this part of the lab later. You can also check on the progress by selecting **State Manager** from the navigation menu.

    - Click on **Inventory** in the navigation menu

    > The application is written in Ruby. We can easily find out which version of Ruby is installed by using the information in the inventory. 

    - Scroll to the bottom and click on any *Instance ID* with **Lab App host** as the name
    - Click on the **Inventory** tab at the top
    - Leave *Inventory type* as **AWS:Application**
    - Search for **Name : Equal : ruby**
    - This will display information about the Ruby installation:
    ![Inventory](../../images/inventory.png)

> You will see information displayed about the version of Ruby you have installed. You have improved your knowledge of the environment and increased your **awareness** of your workload.