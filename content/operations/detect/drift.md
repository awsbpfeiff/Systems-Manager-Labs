---
weight: 2
title: "Drift Detection"
---

1. Navigate to CloudFormation
    - Click [here](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **CloudFormation** in the *AWS Services search box*
        - Select **CloudFormation**

1. Detect Drift on the Lab primary stack

    - You should see all 8 stacks, 7 of them nested
    - Click on the **Lab** primary stack
    - Select the **Stack Actions** dropdown menu
    - Click **Detect Drift**
    - Click on **Drifts** in the navigation menu
    - Note that the stack is currently *IN_SYNC* despite changes we have made

    > You have to perform drift detection on the nested stack. Drift detection on the parent will not discover drift in thge nested stacks.

1. Detect Drift on the SecurityGroups nested stack

    - Click **Stacks** from the left nav or the breadcrumbs
    - Select the nested stack beginning **Lab-SecurityGroups-** 
    - Select the **Stack Actions** dropdown menu
    - Click **Detect Drift**
    - Click on **Drifts** in the navigation menu
    - Refresh the Drifts page if necessary
    - You will note that the Security Group stack shows as drifted because the security groups were updated earlier in the lab
    - Click the logical ID radio button of *LoadBalancerSecurityGroup*
    - Click the **View Drift Details** button

    ![Drift Detection](../../images/drift-detection.png)

> The results screen will show you what has changed from the original CloudFormation template. This should either be reverted or added to the CloudFormation template and pushed as a stack update if the drift was intended (as in this case).

