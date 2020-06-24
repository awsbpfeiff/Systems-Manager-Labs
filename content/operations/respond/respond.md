---
weight: 3
title: "Alert and Automate"
---

> Now you know that uploading a bad image causes an issue on your site and correlates with a log entry containing `"ActiveStorage::InvariableError"`, you can now use this information to create a metric from the logs, alert on that metric and respond automatically to resolve the issue in seconds.

1. Navigate to CloudWatch Logs
    - Click [here](https://eu-west-1.console.aws.amazon.com/cloudwatch/home?region=eu-west-1#logs:) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **CloudWatch** in the *AWS Services search box*
        - Select **CloudWatch**
        - Select **Log groups** from the navigation menu

1. Create Metric

    - Select the **application.log** log group
    - Click on the **Create Metric Filter** button
    - Enter `"ActiveStorage::InvariableError"` (with the quotation marks) for *Filter Pattern*
    - Click on the **Assign Metric** button
    - Enter **ImageError** as the *Metric Name*
    - Click on the **Create Filter** button

1. Create Alarm

    - Click on **Create Alarm**
    - Change *Period* to **10 seconds**
    - Select **Static** for *Threshold type*
    - Select **Greater** for *Whenever ImageError is...*
    - Select **0** under *than...* for the *threshold value*
    - Expand **Additional Configuration**
    - Select **Treat missing data as good (not breaching threshold)** for *Missing data treatment*
    - Click on the **Next** button
    - Select **Select an existing SNS topic** for *Select an SNS topic*
    - Select **Lab-Monitoring...** for *Send a notification to…*

    > You will notice that there is a pre-existing endpoint (**arn:aws:lambda:eu-west-1...**) defined under *Email (endpoints)*. This is because the lab setup created an SNS topic linked to a Lambda function that automatically resolve the issue.

    - Click on the **Next** button
    - Enter **ImageErrorAlarm** for *Define a unique name*
    - Click on the **Next** button
    - Click on the **Create alarm** button
    - The alarm will temporarily be in *INSUFFICIENT* status
    - The alarm will transition to *OK* automatically

1. Navigate to Lambda
    - Click [here](https://eu-west-1.console.aws.amazon.com/lambda/home?region=eu-west-1#/functions) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Lambda** in the *AWS Services search box*
        - Select **Lambda**

1. View the automation
    - Click on the **Lab-Monitoring-...** under *Function name*

    > You can see that the function is triggered by the SNS topic that you selected when you created your alarm.

    ![Lambda](../../images/lambda.png)

    > You can also see the code that deletes the offending image in the database.

    ```
    var mysql = require('mysql');

    var connection = mysql.createConnection({
        host     : process.env.endpoint,
        user     : process.env.user,
        password : process.env.password,
        database : process.env.database
    });

    connection.connect();

    exports.handler = function(event, context) {

        var message = event.Records[0].Sns.Message;
        console.log('Message received from SNS:', message); 
        
        var sql = "DELETE images, active_storage_blobs FROM images,active_storage_blobs WHERE images.id = active_storage_blobs.id AND active_storage_blobs.content_type NOT LIKE 'image/%'";

        connection.query(sql, function(err, results) { 
            if (err){
                console.log(sql);
                console.log("ERROR");
                console.log(err);
                return;
            }
            console.log("Rows Deleted:", results.affectedRows);
            context.succeed('Success');
        });
    
    };
    ```

> You have now created an alarm and linked it to a Lambda function via an SNS Topic to **automate** the response to an event that causes downtime. You should now be able to recover automatically from this outage with seconds of downtime.

