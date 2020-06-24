---
weight: 5
title: "Create Environment"
---

#### Create an AppConfig Deployment Environment

For each application, you define one or more environments. An environment is a logical deployment group of AppConfig applications, such as applications in a `Beta` or `Production` environment. You can also define environments for application subcomponents such as the `Web`, `Mobile`, and `Back-end` components for your application. You can configure Amazon CloudWatch alarms for each environment. The system monitors alarms during a configuration deployment. If an alarm is triggered, the system rolls back the configuration.

In this example, we will create a Development environment for a Lambda supporting an API Gateway Application.

1. Select **Create environment**.

2. Name the environment **AppConfigLabLambdaDevelopment** and provide an optional Description. ![](../images/appconfig-environment.png)

3. Select **Create environment**. ![](../images/appconfig-environment-created.png)

4. Select **AppConfigLab** from the breadcrumb menu to return to the list of environments. ![](../images/appconfig-environment-breadcrumb.png)
