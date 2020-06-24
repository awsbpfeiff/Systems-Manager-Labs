---
weight: 3
title: "Systems Manager Patching"
---

1. Navigate to Systems Manager Patch Manager
    - Click [here](https://eu-west-1.console.aws.amazon.com/systems-manager/patch-manager?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Systems Manager** in the *AWS Services search box*
        - Select **Systems Manager**
        - Select **Patch Manager** from the navigation menu

1. Scan for Patching Compliance

    - Click the **Configure patching** button


    - Specify instances by selecting **Enter instance tags**
    - Enter **Name** for *Tag key*
    - Enter **Lab App host** for *Tag Value* - Case Sensitive
    - Click **Add** - If you don't click add, this won't work
    - Select **Skip scheduling and patch instances now**
    - Select **Scan only** under *Patching operation*
    - Click **Configure patching** button
    - Click **View details button at the top**

    > You should now see two instances being scanned for patches. If you don't see any targets, repeat the previous steps and ensure the tags match your instances.

    - Click on the <i class="fas fa-redo"></i> **refresh** icon in the menu near the top
    - Wait until the *Command status* changes to *Success*

1. Check Compliance Status   
    - Select **Compliance** from the navigation menu
    - You should see that you have two Non-Compliant resources
    - Click on the **2** to apply a filter to the *Resource* list below
    - Click on an instance **ID** in the *Resource* list
    - Search for **State : Missing**    

> You will see information displayed about the missing patches aligned to the default patching baselines for Amazon Linux 2. For the default baseline, all instances should have Critical and Important Security fixes and all Bugfixes older than 7 days. You now have information to apply patches via Patch Manager if required. You now have **awareness** of your workload's patching status and the ability to resolve an issues.

![Compliance Dashboard](../../images/patch-compliance.png)

