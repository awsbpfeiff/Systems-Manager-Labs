---
weight: 1
title: "Identify Issues"
---

{{% notice info %}}
If you skipped the setup section or didn't take note of the URL earlier:
&nbsp;  
&bull;&nbsp;Click [here](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1) or: 
&nbsp;  
&bull;&nbsp;Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
&nbsp;  
&bull;&nbsp;Start typing **CloudFormation** in the *AWS Services search box*
&nbsp;  
&bull;&nbsp;Select **CloudFormation**
&nbsp;  
&bull;&nbsp;You should see all 8 stacks, 7 of them nested
&nbsp;  
&bull;&nbsp;Click on the **Lab** primary stack (the only one without *NESTED*)
&nbsp;  
&bull;&nbsp;Navigate to the **Outputs** tab and open the *ImagetrendsAppUrl* in a new tab
{{% /notice %}}

1. Create an Issue

    - Log in to *ImageTrends* using the URL that you made a note of earlier (CloudFormation Output)
    - Enter **admin@admin.com** for *email*
    - Enter **Password123** for *Password*    
    - Click **Login**
    - Click **Upload Image**
    - Click **Select Image**
    - Find **fake_pic.jpg** in *sample-photos/Break App Photos*
    - Click **Upload**

2. Fix the Issue

    - You will see an error linked to a redirection loop in your browser

    ![Broken Website](../../images/broken.png)

    - **Time to call the devs!**

    > **Ops:** Users are complaining that the site is not accessible. We are seeing a redirect error<br />
    > **Devs:** Oh yeah, we saw this in testing, but we didn't think it would happen in production<br />
    > **Ops:** How can we fix it?<br />
    > **Devs:** Just add **/admin** to the URL and click on images in the menu and delete the image with the error. Sorry got to go, we are busy working on new features..

    - Add **/admin** to the URL
    - Click on **Images** in the menu
    - Delete the image that was last uploaded by clicking on the <i class="fas fa-times"></i> **delete** icon
    - Click **Yes, I'm Sure**
    - Click **/Home**/ on the navigation bar at the top
    - The application is working again.