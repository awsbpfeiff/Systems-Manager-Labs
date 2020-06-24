---
weight: 3
title: "Anomaly Detection"
---

> When you enable anomaly detection for a metric, CloudWatch applies statistical and machine learning algorithms. These algorithms continuously analyze metrics of systems and applications, determine normal baselines, and surface anomalies with minimal user intervention.

1. Navigate to CloudWatch Metrics
    - Click [here](https://eu-west-1.console.aws.amazon.com/cloudwatch/home?region=eu-west-1#metricsV2:graph=~()) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **CloudWatch** in the *AWS Services search box*
        - Select **CloudWatch**
        - Select **Metrics** from the navigation menu

1. Create Metric

    - Click on **ApplicationELB**
    - Click on **Per AppELB Metrics**
    - Select **HTTPCode_Target_2XX_Count • 1 model (Sum)** checkbox
    - Click on the **Graphed metrics** tab
    - If the line chart has gaps, consider changing the *period* to **1 Hour** or another value
    - Click on the <img src="../../images/prediction-icon.svg" alt="Anomaly Detection Icon" style="margin:0px; display: inline-block;"> **Anomaly Detection** icon

1. Create an alarm based on anomalous behaviour    
    - Click on the <i class="far fa-bell"></i> alarm icon under *actions*
    - Click **Next**
    - Select **Create new topic**
    - Enter **lab-hits-topic** for *topic name*
    - Enter **[your email address]** or **example@example.com** as an *email address*
    - Click **Create topic**
    - Click **Next** 
    - Enter **lab-hits-alarm** for *Alarm name*
    - Click **Next** 
    - Click **Create alarm**
    - Click on **lab-hits-alarm**
    - The alarm will intially show *Insufficient data*
    - Generate more traffic to your site by uploading more images and navigating around the site

> Once you have generated a sufficient amount of anomalous traffic (it shouldn't take much), the CloudWatch Alarm will move to a sate of alarm. This is a fairly unrealist scenario because you do not have enough data to provide good training data to the model (as the application has only just been created) and you only have yourself as a user. However, you have used (CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html), which utilises machine learning to help you **detect** possible issues based on the number of successful page hits to your website.

![Anomaly Alarm](../../images/anomaly-alarm.png)



