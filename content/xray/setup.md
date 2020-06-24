---
weight: 1
title: Setup
---

#### Getting Started with AWS X\-Ray<a name="xray-gettingstarted"></a>

To get started with AWS X\-Ray, launch a sample app in Elastic Beanstalk that is already [instrumented](xray-sdk-java.md) to generate trace data\. In a few minutes, you can launch the sample app, generate traffic, send segments to X\-Ray, and view a service graph and traces in the AWS Management Console\.

This tutorial uses a [sample Java application](xray-scorekeep.md) to generate segments and send them to X\-Ray\. The application uses the Spring framework to implement a JSON web API and the AWS SDK for Java to persist data to Amazon DynamoDB\. A servlet filter in the application instruments all incoming requests served by the application, and a request handler on the AWS SDK client instruments downstream calls to DynamoDB\.

![\[Scorekeep sample application flow\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-flow.png)

You use the X\-Ray console to view the connections among client, server, and DynamoDB in a service map\. The service map is a visual representation of the services that make up your web application, generated from the trace data that it generates by serving requests\.

![\[Viewing the connections among client, server, and DynamoDB in a service map\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-servicemap-after.png)

With the X\-Ray SDK for Java, you can trace all of your application's primary and downstream AWS resources by making two configuration changes:
+ Add the X\-Ray SDK for Java's tracing filter to your servlet configuration in a `WebConfig` class or `web.xml` file\.
+ Take the X\-Ray SDK for Java's submodules as build dependencies in your Maven or Gradle build configuration\.

You can also access the raw service map and trace data by using the AWS CLI to call the X\-Ray API\. The service map and trace data are JSON that you can query to ensure that your application is sending data, or to check specific fields as part of your test automation\.

**Topics**
+ [Prerequisites](#xray-gettingstarted-prereqs)
+ [Deploy to Elastic Beanstalk and Generate Trace Data](#xray-gettingstarted-deploy)
+ [View the Service Map in the X\-Ray Console](#xray-gettingstarted-console)
+ [Configuration Amazon SNS Notifications](#xray-gettingstarted-notifications)
+ [Explore the Sample Application](#xray-gettingstarted-sample)
+ [Clean Up](#xray-gettingstarted-cleanup)
+ [Next Steps](#xray-gettingstarted-nextsteps)

#### Prerequisites<a name="xray-gettingstarted-prereqs"></a>

This tutorial uses Elastic Beanstalk to create and configure the resources that run the sample application and X\-Ray daemon\. If you use an IAM user with limited permissions, add the [Elastic Beanstalk managed user policy](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.iam.managed-policies.html) to grant your IAM user permission to use Elastic Beanstalk, and the `AWSXrayReadOnlyAccess` managed policy for permission to read the service map and traces in the X\-Ray console\.

Create an Elastic Beanstalk environment for the sample application\. If you haven't used Elastic Beanstalk before, this will also create a service role and instance profile for your application\.

**To create an Elastic Beanstalk environment**

1. Open the Elastic Beanstalk Management Console with this preconfigured link: [https://console\.aws\.amazon\.com/elasticbeanstalk/\#/newApplication?applicationName=scorekeep&solutionStackName=Java](https://console.aws.amazon.com/elasticbeanstalk/#/newApplication?applicationName=scorekeep&solutionStackName=Java)

1. Choose **Create application** to create an application with an environment running the Java 8 SE platform\.

1. When your environment is ready, the console redirects you to the environment Dashboard\.

1. Click the URL at the top of the page to open the site\.

The instances in your environment need permission to send data to the AWS X\-Ray service\. Additionally, the sample application uses Amazon S3 and DynamoDB\. Modify the default Elastic Beanstalk instance profile to include permissions to use these services\.

**To add X\-Ray, Amazon S3 and DynamoDB permissions to your Elastic Beanstalk environment**

1. Open the Elastic Beanstalk instance profile in the IAM console: [aws\-elasticbeanstalk\-ec2\-role](https://console.aws.amazon.com/iam/home#roles/aws-elasticbeanstalk-ec2-role)\.

1. Choose **Attach Policy**\.

1. Attach **AWSXrayFullAccess**, **AmazonS3FullAccess**, and **AmazonDynamoDBFullAccess** to the role\.

#### Deploy to Elastic Beanstalk and Generate Trace Data<a name="xray-gettingstarted-deploy"></a>

Deploy the sample application to your Elastic Beanstalk environment\. The sample application uses Elastic Beanstalk configuration files to configure the environment for use with X\-Ray and create the DynamoDB that it uses automatically\.

**To deploy the source code**

1. Download the sample app: [eb\-java\-scorekeep\-xray\-gettingstarted\-v1\.3\.zip](https://github.com/awslabs/eb-java-scorekeep/releases/download/xray-gs-v1.3/eb-java-scorekeep-xray-gettingstarted-v1.3.zip)

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management console](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) for your environment\.

1. Choose **Upload and Deploy**\.

1. Upload eb\-java\-scorekeep\-xray\-gettingstarted\-v1\.3\.zip, and then choose **Deploy**\.

The sample application includes a front\-end web app\. Use the web app to generate traffic to the API and send trace data to X\-Ray\.

**To generate trace data**

1. In the environment Dashboard, click the URL to open the web app\.

1. Choose **Create** to create a user and session\.

1. Type a **game name**, set the **Rules** to **Tic Tac Toe**, and then choose **Create** to create a game\.

1. Choose **Play** to start the game\.

1. Choose a tile to make a move and change the game state\.

Each of these steps generates HTTP requests to the API, and downstream calls to DynamoDB to read and write user, session, game, move, and state data\.

#### View the Service Map in the X\-Ray Console<a name="xray-gettingstarted-console"></a>

You can see the service map and traces generated by the sample application in the X\-Ray console\.

**To use the X\-Ray console**

1. Open the service map page of the [X\-Ray console](https://console.aws.amazon.com/xray/home#/service-map?timeRange=PT1H)\.

1. The console shows a representation of the service graph that X\-Ray generates from the trace data sent by the application\.  
![\[Representation of service graph X-Ray generates from trace data sent by the application\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-servicemap-before.png)

The service map shows the web app client, the API running in Elastic Beanstalk, the DynamoDB service, and each DynamoDB table that the application uses\. Every request to the application, up to a configurable maximum number of requests per second, is traced as it hits the API, generates requests to downstream services, and completes\.

You can choose any node in the service graph to view traces for requests that generated traffic to that node\. Currently, the Amazon SNS node is red\. Drill down to find out why\.

**To find the cause of the error**

1. Choose the node named **SNS**\.

1. Choose the trace from the **Trace list**\. This trace doesn't have a method or URL because it was recorded during startup instead of in response to an incoming request\.  
![\[Choosing a trace from the Trace list\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-tracelist-sns.png)

1. Choose the red status icon to open the **Exceptions** page for the SNS subsegment\.  
![\[Choosing the red status icon opens the Exceptions page for the SNS subsegment\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-timeline-sns.png)

1. The X\-Ray SDK automatically captures exceptions thrown by instrumented AWS SDK clients and records the stack trace\.  
![\[Exceptions tab showing captured exceptions and recorded stack trace\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-exception.png)

The cause indicates that the email address provided in a call to `createSubscription` made in the `WebConfig` class was invalid\. Let's fix that\.

#### Configuration Amazon SNS Notifications<a name="xray-gettingstarted-notifications"></a>

Scorekeep uses Amazon SNS to send notifications when users complete a game\. When the application starts up, it tries to create a subscription for an email address defined in an environment variable\. That call is currently failing, causing the Amazon SNS node in your service map to be red\. Configure a notification email in an environment variable to enable notifications and make the service map green\.

**To configure Amazon SNS notifications for Scorekeep**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management console](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) for your environment\.

1. Choose **Configuration**\.

1. Choose **Software Configuration**\.

1. Under **Environment Properties**, replace the default value with your email address\.  
![\[Setting environment properties\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-beanstalk-envvars.png)
**Note**  
The default value uses an AWS CloudFormation [function](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions-functions.html) to retrieve a parameter stored in a configuration file \(a dummy value in this case\)\.

1. Choose **Apply**\.

When the update completes, Scorekeep restarts and creates a subscription to the SNS topic\. Check your email and confirm the subscription to see updates when you complete a game\.

![\[Service map after updates\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-servicemap-after.png)