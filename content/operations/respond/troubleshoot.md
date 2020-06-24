---
weight: 2
title: "Troubleshoot"
---

> You know from Systems Manager Inventory that the application uses Ruby 2.4.4, so it's possible to determine the liekly log format. A quick Internet search reveals that the [Ruby Logger class documentation](https://ruby-doc.org/stdlib-2.4.4/libdoc/logger/rdoc/Logger.html). From this document, you can determine that the log format is as follows: 
>
> `SeverityID, [DateTime #pid] SeverityLabel -- ProgName: message`
>
> This information helps to make the logs more useable in CloudWatch Logs Insights.

1. Navigate to CloudWatch Insights
    - Click [here](https://eu-west-1.console.aws.amazon.com/cloudwatch/home?region=eu-west-1#logs-insights:) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **CloudWatch** in the *AWS Services search box*
        - Select **CloudWatch**
        - Select **Insights** from the navigation menu

1. Investigate Log Severity Information

    - Select **application.log** from the dropdown menu at the top
    - Enter the following query in to the query textarea

    ```
    parse @message "*, [* #*] * -- *: *" as SeverityID, 
        DateTime, PID, SeverityLabel, ProgName, Message
    | stats count(*) as Count by SeverityLabel, bin(5m) as Time
    | sort @timestamp
    | limit 100
    ```

    > `parse @message "*, [* #*] * -- *: *" as SeverityID...` parses the application log in to fields that CloudWatch Logs Insights can use, using the log format from Ruby.<br />
    > `| stats count(*) as Count by SeverityLabel, bin(5m) as Time` groups the results by severity and 5 minute intervals<br />
    > `| sort @timestamp` sorts by date and time<br />
    > `| limit 100` limits thre results to 100 rows

    - Click on **Run query**

    ![CloudWatch Insights Logs by Severity](../../images/severity.png)

    > So far, you have discovered that the application log does contain some errors. Now it's time to dive in to the errors. 
    
1. Investigate the errors

    - Replace the following query in to the query textarea

    ```
    parse @message "*, [* #*] * -- *: *" as SeverityID, 
        DateTime, PID, SeverityLabel, ProgName, Message
    | filter SeverityLabel="ERROR"
    | sort @timestamp
    | limit 100
    ```

    > `| filter SeverityLabel="ERROR"` filters the results to only show errors<br />

    - Click on **Run query**
    - Expand the first error

    ![CloudWatch Insights Logs by Severity](../../images/error.png)

    > A quick internet search reveals that **ActiveStorage::InvariableError** is raised if ImageMagick cannot transform the blob. See [here](https://api.rubyonrails.org/classes/ActiveStorage/Blob/Representable.html) for more information. This makes sense, as the image you uploaded was not a valid image, so there is no way it could be transformed by ImageMagick. You have used CloudWatch Insights to **Identify** the cause of the issue and correlate it to a log entry. Now it's possible to alert and respond to this event.





