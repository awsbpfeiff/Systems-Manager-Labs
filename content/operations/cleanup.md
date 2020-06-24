---
weight: 5
title: Lab Cleanup
---

> Before deleting the stack, you must first remove some resources.

- Delete S3 Bucket
    - Navigate to the **S3** Console
    - Select the **lab-imageuploadbucket-...** bucket and click the **Empty** button
    - Copy and paste the bucket name into the dialog box
    - Click **Confirm**

- Delete Additional RDS Instance
    - Navigate to the **RDS** Console
    - Click DB Instances (2/40) and select the Reader instance
    - From the *Action* menu choose **delete**
    - Type **delete me** in to the field
    - Click **Delete**

- Delete the Stack
    - Navigate to the **CloudFormation** console
    - Select the base **lab** stack radio button
    - Click the **Delete** button
    - Click the **Delete Stack** button

- Delete CloudWatch Items
    Navigate to CloudWatch and delete:
    - **lab-login-canary** in *Canaries* (*stop* then *delete*)
    - **/aws/lambda/cwsyn-lab-login-canary-...** in *Log groups*
    - **/aws/lambda/Lab-Monitoring-..** Log Group in *Log groups*
    - **application.log** Log Group in *Log groups*
    - **boot.log** Log Group in *Log groups*
    - **messages** Log Group in *Log groups*
    - **production.log** Log Group in *Log groups*
    - **ImageErrorAlarm** in *Alarms*
    - **lab-hits-alarm** in *Alarms*
    - **Synthetics-Alarm-lab-login-canary** in *Alarms*
    - **All > ApplicationELB > Per AppELB Metrics > ActiveConnectionCount > Anomaly Detection** in *metrics*
   
Navigate to GuardDuty, select Settings in the navigation menu and choose to Suspend GuardDuty or Disable GuardDuty
