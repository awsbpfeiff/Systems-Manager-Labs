---
weight: 1
title: "Narrative"
---

#### 

AWS AppConfig can be used to create, manage, and quickly deploy application configurations. AppConfig supports controlled deployments to applications of any size and includes built-in validation checks and monitoring. You can use AppConfig with applications hosted on EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices.

To prevent errors when deploying application configurations, especially for production systems where a simple typo could cause an unexpected outage, AppConfig includes validators. A validator provides a syntactic or semantic check to ensure that the configuration you want to deploy works as intended. To validate your application configuration data, you provide a schema or Lambda function that runs against the configuration. The configuration deployment or update can only proceed when the configuration data is valid.

During a configuration deployment, AppConfig monitors the application to ensure that the deployment is successful. If the system encounters an error, AppConfig rolls back the change to minimize impact for your application users. You can configure a deployment strategy for each application or environment that includes deployment criteria, including velocity, bake time, and alarms to monitor. Similar to error monitoring, if a deployment triggers an alarm, AppConfig automatically rolls back to the previous version.

AppConfig supports multiple use cases. Here are some examples.

* **Application tuning:** Use AppConfig to carefully introduce changes to your application that can only be tested with production traffic.
* **Feature toggle:** Use AppConfig to turn on new features that require a timely deployment, such as a product launch or announcement.
* **User membership:** Use AppConfig to allow premium subscribers to access paid content.
* **Operational issues:** Use AppConfig to reduce stress on your application when a dependency or other external factor impacts the system.

In this lab, we will go over the Feature toggle use case and see AppConfig in action.
