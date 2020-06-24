---
weight: 3
title: "Metrics"
---

#### Creating CloudWatch Metric Filters

You can use metric filters to monitor events in a log group as they are sent to CloudWatch Logs. You can monitor and count specific terms or extract values from log events and associate the results with a metric.  In this section, we will manually create a Metric Filter to show access denied errors found in CloudTrail.

1. Go to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home#logs:)
2. Select the radial button of the CloudTrail Log Group we created in the setup.
3. Once selected, click on Create Metric Filter.
4. In the Filter pattern field, copy and paste the following query.
    ```
    {$.errorCode = "ValidationException" || $.errorCode = "AccessDenied"}
    ```
5. Click on Test Pattern to make sure the filter is working.
6. Click on Assign Metric.
7. In the Create Metric Filter and Assing a Metric page, enter the following values.
 - Filter Name: give a name or leave the default text displayed
 - Metric Namespace:  leave the default **LogMetrics** or give a name
 - Metric Name: AccessError
8. Click on Create Filter


#### Graphing Creating CloudWatch Metric Filters

In the previous section, we created a Metric Filter from the CloudTrail events in CloudWatch Logs.  In this section, we will visualize the data in a graph based on that filter.

1. Go to the [CloudWatch Metrics console]('https://console.aws.amazon.com/cloudwatch/home#metricsV2:graph=~(metrics~(~(~'CloudTrailMetrics~'AuthorizationFailureCount~(period~300~visible~false)))~view~'timeSeries~stacked~false~region~'us-east-1~stat~'Average)').
2. Click on the All metrics tab.
3. Select LogMetrics.
4. Click on Metriics with no dimensions.
5. Check the AccessError check box.

    ![Metric Filter](../images/AccessError.png) 

6. Make sure 1d is selected for the time span.

In the graph, we will see the activity related to the access denied errors found in the CloudTrail events of the CloudWatch Log Group.

![Access Error Graphed in CloudWatch Metrics](../images/AccessErrorGraph.png) 


**End of Lab Exercises** 
 
**Thank you for using this lab.** 