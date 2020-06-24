---
weight: 1
title: "Trusted Advisor"
---

{{% notice info %}}
If your account has been provisioned for you by AWS for the purpose of completing this lab, you will not be able to see any recommendations from Trusted Advisor. You can still complete the remediation activities.
{{% /notice %}}

1. Navigate to Trusted Advisor
    - Click [here](https://console.aws.amazon.com/trustedadvisor/home?region=eu-west-1#/dashboard) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Trusted Advisor** in the *AWS Services search box*
        - Select **Trusted Advisor**
    - Wait for account to <i class="fas fa-sync-alt"></i> **refresh** ( spinning icon in the top right)

1. Security

    **Business or Enterprise Support plans:** If your account was empty, you will see some Security and Fault Tolerance warnings and errors

    **Basic or Developer Support plans:** If your account was empty, you will only see security errors and warnings.
    
    - Click on **Security**

        > **IAM Password Policy** and **MFA on Root Account** are two items that you should take care of outside of this lab to ensure security best practice for your account.
        >
        >   - [Setting an Account Password Policy for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html)
        >   - [Using Multi-Factor Authentication (MFA) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)    

    - If you have **Business or Enterprise support:**
        - Expand *Security Groups - Unrestricted Access* and you'll see that we have a security group with unrestricted access to ports other than 25,80 or 443.
        - Click on the *Security Group Name*

    - If you have **Basic or Developer support or can't access to Trusted Advisor:**
        - Click **Services** in the navigation bar
        - Click on **EC2**
        - Select **Security Groups** in the navigation menu
        - Select the *Security Group* with the *Name* **Lab-LoadBalancer** 

    - Everyone continue: 
        - Click on **Inbound**
        - Click **Edit**
        - Change *Type* to **HTTP**
        - Click **Save**

    - Navigate back to Trusted Advisor:
        - Click [here](https://console.aws.amazon.com/trustedadvisor/home?region=eu-west-1#/dashboard) or: 
            - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
            - Start typing **Trusted Advisor** in the *AWS Services search box*
            - Select **Trusted Advisor**
        - Click on the <i class="fas fa-sync-alt"></i> **refresh** icon in the top right

        > You will see that your **security** warnings have decreased by one. You will also see the the change you have made under *Recent Changes*. You have improved your security posture by helping to **prevent** an attack.

1. Fault Tolerance

    - If you have **Business or Enterprise support:**
        - Click on **Fault Tolerance**
        - Expand *Amazon RDS Multi-AZ*
        - You will see that *Trusted Advisor* has identified a risk due to a lack of Multi-AZ. 

    - Everyone continue:
        - Click [here](https://console.aws.amazon.com/rds/home?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **RDS** in the *AWS Services search box*
        - Select **RDS**
        - Click **Recommendations**
        - Click to expand **Single instance DB cluster (1)**
        - Select the checkbox next to the resource beginning **lab-database**
        - Click **Apply Now**
        - Click **Confirm**
        - Click **View Resource** at the top of the page

        > You will see that a new *Reader* exists with a status of *creating*. When the instance has finished creating, you will have created an additional instance in another availability zone. You have improved your fault tolerance by helping to **prevent** extended downtime. After you have remediated these issues, a Trusted Advisor Dashboard with Enterprise Support will show the changes made.

![Trusted Advisor Dashboard](../../images/trusted-advisor-dashboard.png)
