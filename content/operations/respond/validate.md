---
weight: 4
title: "Validate"
---

1. Recreate the Issue

    - Log in to *ImageTrends* using the URL that you made a note of earlier (CloudFormation Output)
    - Enter **admin@admin.com** for *email*
    - Enter **Password123** for *Password*    
    - Click **Login**
    - Click **Upload Image**
    - Click **Select Image**
    - Find **fake_pic.jpg** in *sample-photos/Break App Photos*
    - Click **Upload**

1. Navigate to Lambda
    - Click [here](https://eu-west-1.console.aws.amazon.com/lambda/home?region=eu-west-1#/functions) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Lambda** in the *AWS Services search box*
        - Select **Lambda**
        - Click on **Applications** in the menu

1. View the results
    - Click on the **Lab-Monitoring-...** under *Name*
    - Click on the **Monitoring** tab
    - View the invocations
    - See that your website has recovered!

    ![Invocations](../../images/invocations.png)

> You should see an invocation of your lambda function, indicating that the function has run. You may notice that your website has recovered before the invocation appears in the user interface. 





