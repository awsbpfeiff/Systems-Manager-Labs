---
weight: 4
title: "Install Cloudwatch"
---

> Additional logs and metrics can be capture by installing and configuring the CloudWatch agent.

1. Navigate to Systems Manager Parameter Store
    - Click [here](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters?region=eu-west-1) or: 
        - Navigate to the [AWS Console](https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1)
        - Start typing **Systems Manager** in the *AWS Services search box*
        - Select **Systems Manager**
        - Select **Parameter Store** from the navigation menu
        - Copy the *Name* to use later

1. View the CloudWatch Configuration

    - Click on the *Name* to view the details 

    > We have created a parameter that will be used to configure the CloudWatch agent for the application. The code is displayed below for easy reading.


    ```

        {
            "logs": {
                "logs_collected": {
                    "files": {
                        "collect_list": [{
                            "file_path": "/var/log/messages",
                            "log_group_name": "messages"
                        }, {
                            "file_path": "/var/log/demsg",
                            "log_group_name": "dmesg"
                        }, {                    
                            "file_path": "/var/log/boot.log",
                            "log_group_name": "boot.log"
                        }, {
                            "file_path": "/opt/imagetrends/log/application.log",
                            "log_group_name": "application.log"
                        }, {
                            "file_path": "/opt/imagetrends/log/production.log",
                            "log_group_name": "production.log"
                        }]
                    }
                }
            },
            "metrics": {
                "append_dimensions": {
                    "AutoScalingGroupName": "${aws:AutoScalingGroupName}",
                    "ImageId": "${aws:ImageId}",
                    "InstanceId": "${aws:InstanceId}",
                    "InstanceType": "${aws:InstanceType}"
                },
                "metrics_collected": {
                    "cpu": {
                        "measurement": ["cpu_usage_idle", "cpu_usage_iowait", "cpu_usage_user", "cpu_usage_system"],
                        "metrics_collection_interval": 60,
                        "resources": ["*"],
                        "totalcpu": false
                    },
                    "disk": {
                        "measurement": ["used_percent", "inodes_free"],
                        "metrics_collection_interval": 60,
                        "resources": ["*"]
                    },
                    "diskio": {
                        "measurement": ["io_time"],
                        "metrics_collection_interval": 60,
                        "resources": ["*"]
                    },
                    "mem": {
                        "measurement": ["mem_used_percent"],
                        "metrics_collection_interval": 60
                    },
                    "statsd": {
                        "metrics_aggregation_interval": 60,
                        "metrics_collection_interval": 10,
                        "service_address": ":8125"
                    },
                    "swap": {
                        "measurement": ["swap_used_percent"],
                        "metrics_collection_interval": 60
                    }
                }
            }
        }
    ```

1. Install CloudWatch

    - Click **Run Command** in the navigation menu
    - Click **Run command** button
    - Select **AWS-ConfigureAWSPackage**
    - In the *Action* list, choose **Install**.
    - In the *Name* field, type **AmazonCloudWatchAgent**
    - Under *Targets* select **Specify instance tags**
    - Enter **Name** for *Tag key*
    - Enter **Lab App host** for *Tag Value*
    - Click **Add**
    - Uncheck **Enable writing to an S3 bucket** under *Output options*
    - Click **Run** button
    - Click on the <i class="fas fa-redo"></i> **refresh** icon in the menu near the top
    - Wait until the *Command status* changes to *Success*

    > You should now see two instances having CloudWatch installed. If you don't see any targets, repeat the previous steps and ensure the tags match your instances.

    ![Inventory](../../images/install-cloudwatch.png)

1. Configure CloudWatch

    - Click **Run Command** in the navigation menu
    - Click **Run command** button
    - Search Document name **prefix : Equal : AmazonCloudWatch-ManageAgent**
    - Select **AmazonCloudWatch-ManageAgent** radio button
    - In the *Optional Configuration Location* enter **[the name you copied from parameter store]**
    - Under *Targets* select **Specify instance tags**
    - Enter **Name** for *Tag key*
    - Enter **Lab App host** for *Tag Value*
    - Click **Add**
    - Uncheck **Enable writing to an S3 bucket** under *Output options*
    - Click **Run** button
    - Click on the <i class="fas fa-redo"></i> **refresh** icon in the menu near the top
    - Wait until the *Command status* changes to *Success*

    > You should now see two instances having CloudWatch configured. If you don't see any targets, repeat the previous steps and ensure the tags match your instances.

> You have now configured your environment to provide **telemetry** with CloudWatch logging and metrics.