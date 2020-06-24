---
weight: 3
title: "Collect telemetry"
---

#### Implementing the Monitoring Plan: Telemetry
 
Consider: What telemetry can we leverage to achieve the goals? Is there desirable telemetry we do not have? If so, how can we acquire it? Can we further instrument our application to emit the information we need? Can we configure out infrastructure to provide the information we need? 
 
Our workload must emit the data that allows operations to understand workload health and the achievement of business outcomes. We have to collect that information, analyze it, and then act on it to maximize the benefits for our organizations. If we are collecting without analysis or action we are paying to store records that will at best only be used after an event in a forensic capacity. Our goal is to achieve insight. 
 
Categories of insight: 
 
- Faults 
- Configuration 
- Accounting 
- Performance 
- Security 
- Outcomes 
- User Behavior 
- Workload Behavior 
 
We will iterate: engaging our stakeholders, determining requirements and priorities, implementing improvements to monitoring, evaluating their success, and repeating. Or put more simply: create a plan, implement the plan, check to see if the plan works, adjust the plan and repeat. 
 
#### 3 Acquire Telemetry 
 
We are going to start by focusing on 3 goals 
 
- Titus Grone, our application director, wants to know that our production application is delighting our customers. 
- Ryn Brandish, our DevOps Engineer, wants to understand when there are application issue and is concerned about security. 
- Sansa Bailish, our SRE, is focused on outages, reliability, and getting better sleep. 
 
**3.1 Install and configure the CloudWatch agent** 
 
We will collect metrics and logs from our Amazon EC2 instances using the CloudWatch agent. If we had on-prem servers we could also use the CloudWatch agent to capture metrics and logs from those. 
 
The CloudWatch Agent requires and IAM role that was created with the execution of the CloudFormation script. We will install the agent, and then configure it using a predefined configuration placed in System Manager Parameter Store by our CloudFormation script. With our configuration in parameter store we can easily apply it as a standard across our fleet. You can learn more by following this [*Getting Started*](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-first-instance.html) link. 
 
**3.2 Review the Parameter Store configuration** 
 
The configuration file we have in Parameter Store follows a required naming convention (it must start with AmazonCloudWatch-) that allows us to take advantage of the CloudWatchAgentServerPolicy IAM role. To simplify our management. You can learn more at this [link](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html).. 
 
1. Open **Parameter Store** in the System Manager console by clicking on this [link](https://console.aws.amazon.com/systems-manager/parameters). 
2. Choose **AmazonCloudWatch-ExampleCorpConfig** 
3. Review the logs that will be ingested into CloudWatch Logs 
 
**3. Install and Configure the CloudWatch agent Systems Manager Run Command** 
 
Using System Manager's Run Command we can install and configure the CloudWatch agent on an entire fleet as easily as we install it on a single instance. By using a command document we ensure consistent execution and limit the errors that could be introduced by a manual process. 
 
1. Navigate to the Systems Manager console **Run Command** page by clicking on this [link](https://console.aws.amazon.com/systems-manager/run-command). 
2. Choose **Run command**. 
    - In the **Command document** list, select **AWS-ConfigureAWSPackage** by selecting the radio button next to it. 
    - In the **Action** list, choose **Install**. 
    - In the **Name** field, type **AmazonCloudWatchAgent** 
    - In the **Version** field, type *latest*. 
    ![Command Parameters](../images/cwagentinstallcommandparams.jpg) cwagenttargets 
    - In the **Targets** area, Manually select the ExampleCorp instances. You could also target instances by tag. 
    ![Command Parameters](../images/cwagenttargets.jpg) 
    - In the **Output options** area uncheck the box next to **Enable writing to an S3 bucket**. 
    - Choose **Run**. 
3. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**. Systems Manager should show that the agent was successfully installed. 
 
**3.3 Configure and (re)start the CloudWatch agent using Systems Manager Run Command** 
 
1. Open Run Command in the Systems Manager console by clicking on this [link](https://console.aws.amazon.com/systems-manager/run-command). 
2. Choose **Run command**. 
    - In the Command document list, select **AmazonCloudWatch-ManageAgent** by selecting the radio button next to it. 
    - In the **Command Parameters** area 
        - In the **Action** list, choose the default **configure**. 
        - In the **Mode** list, choose the default **ec2** 
        - In the **Optional Configuration Source** list, choose the default ssm. 
        - In the **Optional Configuration Location** box, type the name of the agent configuration file that we created and saved to Systems Manager Parameter Store *AmazonCloudWatch-ExampleCorpConfig* 
        ![Command Parameters](../images/cwagentconfigcommandparams.jpg) 
    - In the **Optional Restart** list, choose **yes** to start the agent after you have finished these steps. 
    - In the **Targets** area, choose the instance where you installed the CloudWatch agent. 
    - In the **Output options** area uncheck the box next to **Enable writing to an S3 bucket**. 
3. Choose **Run**. 
4. Optionally, in the Targets and outputs areas, select the button next to an instance name and choose View output. Systems Manager should show that the agent was successfully started 
 
Want to learn more about collecting logs and metrics using the unified CloudWatch Agent? Follow this [link](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html). 
 
(Optional Exercise) Review the Collected Application Logs [with the development team] 
 
The more integrated the business, development, and operations teams are, the more successful they will be. The custom developed application emits a variety of logs. Engaging the developers to help gain insight to their contents, meaning, and their purpose will shorten the amount of time it takes to get value out of monitoring. 
 
1. Navigate to the CloudWatch Logs dashboard at this [link](https://console.aws.amazon.com/cloudwatch/home#logs:). 
2. Choose application.log from the Log Groups list 
3. Choose the Log Stream of your instance 
4. Review the available logs 
 
**3.4 Generate logs through user activity** 
 
1. In a web browser navigate to the URL Output of the Example Corp CloudFormation Stack to reach the ExampleCorp application  
![CloudFormation Output](../images/cfnurloutput.jpg) 
2. Create an account by choosing login and then choosing Sign up at the Log in page. 
    - Enter an Email address, a Username, and a Password and choose Sign up 
    - There is no validation on the email address you provide so you may use a fake address 
3. Upload some photos 
    - you can either use your own photos or 
    - [**download sample photos here**](https://s3.amazonaws.com/www.awsmanagementweek.com/sample-photos.zip) 
        **Note:** There are 76MB of images 
4. Navigate to the CloudWatch Logs dashboard at this [link](https://console.aws.amazon.com/cloudwatch/home#logs:). 
    - Choose application.log from the Log Groups list 
    - Choose the Log Stream of an instance 
    - At the top right of the grey field, in the time window definition box, choose the pull down. 
    - Choose to Relative, by clicking on the term. 
    - Choose 2 Hours 
    ![CloudFormation Output](../images/applicationlogtime.jpg) 
    - Review the recent logs. Enter a Tag value identified by ExampleCorp in the Filter Events text box and review the associated logs. 
 
**3.5 Publish VPC Flow Logs to CloudWatch Logs** 
 
When [publishing VPC flow logs to CloudWatch Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-cwl.html), flow log data is published to a log group, and each network interface has a unique log stream in the log group. Log streams contain flow log records. You can create multiple flow logs that publish data to the same log group. If the same network interface is present in one or more flow logs in the same log group, it has one combined log stream. If you've specified that one flow log should capture rejected traffic, and the other flow log should capture accepted traffic, then the combined log stream captures all traffic. For more information, see [Flow Log Records](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html#flow-log-records). 
 
Consider how you will use the collected VPC Flow Log data. On internal networks visibility of intended and unintended traffic can facilitate diagnosing issues. Capturing rejected traffic on your Internet Gateway by default may result in storing and processing a lot of data with limited value to you. Remember you can always enable the capability when needed and then disable it when no longer needed to help optimize the [value your get from CloudWatch](https://aws.amazon.com/cloudwatch/pricing/). 
 
**(Optional)** Create a flow log for your VPC 
 
**Note:** The following steps have been completed for you by the CloudFormation script. 
 
1. Open the Amazon VPC console and navigate to Your VPCs by clicking on this [link](https://console.aws.amazon.com/vpc/home?#vpcs:sort=VpcId). 
2. Select your ExampleCorp VPCs and then choose Actions, Create flow log. 
3. For Filter, choose All from the pull down list to log accepted and rejected traffic. 
4. For Destination, choose the default Send to CloudWatch Logs. 
5. For Destination log group, type the name of a log group in CloudWatch Logs to which the flow logs are to be published. If you specify the name of a log group that does not exist, it will attempt to create the log group for you. Enter ExampleCorpVPC in the text box next to Destination log group*. 
6. For IAM role select the role with name in the form \(your stack name\)-FlowLogsRole-\(random string\). It was created by the CloudFormation script with permissions to publish logs to CloudWatch Logs. 
7. Choose Create. 
8. Open the CloudWatch console and navigate to Logs by clicking on this [link](https://console.aws.amazon.com/cloudwatch/home?#logs:). 
 
**What have we accomplished?** 
 
All of our instances are now logging to CloudWatch Logs and, our VPC is logging to CloudWatch Logs as well. Our instances and [AWS services are publishing metrics to CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html). Our application and workload are providing traces though integration with [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html). We are collecting the available telemetry, and now it is time to analyze it and take advantage of it. 
