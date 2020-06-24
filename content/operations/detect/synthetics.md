---
weight: 4
title: "Synthetic Monitoring"
---

> Amazon CloudWatch Synthetics enables you to create canaries to monitor your endpoints and APIs. Canaries are configurable scripts that follow the same routes and perform the same actions as a customer. This enables the outside-in view of your customers’ experiences, and your service’s availability from their point of view.

{{% notice info %}}
If your account has been provisioned for you by AWS for the purpose of completing this lab, you will need to skip this section of the lab.
{{% /notice %}}

1. Navigate to CloudWatch Synthetics
    - Click [here](https://eu-west-1.console.aws.amazon.com/cloudwatch/home?region=eu-west-1#synthetics:) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **CloudWatch** in the *AWS Services search box*
        - Select **CloudWatch**
        - Select **Synthetics** from the navigation menu

1. Create Canary

    - Click on **Create Canary**
    - Select **GUI workflow builder**
    - Enter **lab-login-canary** for *Name*
    - Enter **[Application URL]/users/sign_in** as the *Application or endpoint URL you are testing*
    - Under *Workflow builder:*
        - Choose **Input text** in the *action* dropdown
        - Enter **`[id='user_email']`** in the *selector* input
        - Enter **admin@admin.com** in the *text* input 
        - Click **Add action**
        - Choose **Input text** in the *action* dropdown
        - Enter **`[id='user_password']`** in the *selector* input
        - Enter **Password123** in the *text* input 
        - Click **Add action**
        - Choose **Click with navigation** in the *action* dropdown
        - Enter **`[name='commit']`** in the *selector* input
        - Click **Add action**
        - Choose **Verify Selector** in the *action* dropdown
        - Enter **`[href='/users/sign_out']`** in the *selector* input
    - Toggle **Enable thresholds for this canary**
    - Click **Create canary**

    > **Note:** This operation may take up to a minute. Please be patient while it completes.

    ![Canaries](../../images/canaries.png)

    - Click on **lab-login-canary** 

    > Explore the **Summary**, **Availability** graph and **Screenshots**. Try clicking on **HAR File** to view request information and **Logs** to view execution information. This canary is testing your login process. 
    
1. Generate a failure

    - Change the admin email in the *ImageTrends* application
        - Log in to *ImageTrends* using the URL that you made a note of earlier (CloudFormation Output)
        - Enter **admin@admin.com** for *email*
        - Enter **Password123** for *Password*
        - Select **My Profile** from the drop down in the top right when logged in as admin
        - Change *Email* to something like **admin2@admin.com**
        - Enter **Password123** for *Current password*

1. Create an Alarm

    - Select **Thresholds** from the navigation menu
    - Toggle **Sharp drop**
    - Click **Save**
    - Click **Yes, Save Changes**
    - Wait for the alarm to change status to **Alarm**

1. Resolve the failure

    - Change the admin email back in the *ImageTrends* application
        - Select **My Profile** from the drop down in the top right when logged in as admin
        - Change *Email* back to **admin@admin.com**
        - Enter **Password123** for *Current password*
        - Wait for the alarm to change status to **OK**

1. View the results
    - Select **Canaries** from the navigation menu
    - Click on **lab-login-canary** 
    - You can click on the red dots on the graph to view the **Screenshots**, **HAR File** and **Logs**
    - Screenshot **05-redirection-result** shows the login failure

![Canary Errors](../../images/canary-errors.png)

> You have used [CloudWatch Synthetics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html) to **detect** issues in your application workflow. In this case, you have created a workflow to **detect** issues with the login process from the customer perspective.





