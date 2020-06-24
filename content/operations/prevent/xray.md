---
weight: 5
title: "Tracing with X-Ray"
---

> AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components. You can use X-Ray to analyze both applications in development and in production, from simple three-tier applications to complex microservices applications consisting of thousands of services.

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

1. Generate traffic on the application that has been deployed
    - Log in to *ImageTrends* using the URL that you made a note of earlier (CloudFormation Output).
    - Enter **admin@admin.com** for *email*
    - Enter **Password123** for *Password*
    - Click **Login**
    - Download [Sample Photos](https://workshop.aws-management.tools/operations/templates/sample-photos.zip) 
    - Click **Upload Image**
    - Use one of the images that you downloaded in the *Success Photos* folder
    - Repeat a few times to generate some data

1. Navigate to X-Ray
    - Click [here](https://eu-west-1.console.aws.amazon.com/xray/home?region=eu-west-1#/service-map) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **X-Ray** in the *AWS Services search box*
        - Select **X-Ray**

1. Explore the Service Map

    {{% notice info %}}
Your service Map may not populate immediately, use the refresh icon in the top right or wait for the map to auto refresh.
&nbsp;  
&nbsp;  
If you do not see data that you are expecting, try changing the time from **Last 5 minutes** in the top right to **30** minutes.
    {{% /notice %}}
    ![Deploy Lab](../../images/x-ray-service-map.png)

1. View Traces
    - Click on the **imagetrends** circle
    - Click on the **View traces** button
    - Click on a *Trace list ID*
    - Review the trace details
    - Click **Analytics** in the navigation menu on the left
    - Scroll down to *Response Time Root Cause* 
    - Select **Rekognition (AWS::Rekognition)**
    - Change the time from Last 5 minutes to see further back. 

> Your application has been pre-configured to provide additional tracing **telemetry** with X-Ray.