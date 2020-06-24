---
weight: 7
title: "Other Activities"
---

#### Detect Drift
> Performing a drift detection operation on a stack determines whether the stack has drifted from its expected template configuration, and returns detailed information about the drift status of each resource in the stack that supports drift detection. You can try this out in the next section of this lab.

- Learn more about [detecting drift on a CloudFormation stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html)
- Automate drift detection using [AWS Config cloudformation-stack-drift-detection-check](https://docs.aws.amazon.com/config/latest/developerguide/cloudformation-stack-drift-detection-check.html)

#### Automated Security Assessment
> Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Amazon Inspector automatically assesses applications for exposure, vulnerabilities, and deviations from best practices. After performing an assessment, Amazon Inspector produces a detailed list of security findings prioritized by level of severity.

- Learn more about [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html)

**Try It**

- **Warning:** it is likely to take more than an hour to run the asessment
- Create an *assessment* on the lab by clicking [here](https://eu-west-1.console.aws.amazon.com/inspector/home?region=eu-west-1#/wizard/)  
- Click on **Assessment templates** in the navigation menu
- Select the **Assessment-Template-Default-All-Rules** template
- Click **Run**
- After the assessment is complete, you will see a number of findings
![Inspector Dashboard](../../images/inspector-dashboard.png)

#### Automated Configuration Management
> AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations.

- Learn more about [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html)

**Try It**

- Enable *Config* in the lab [here](https://eu-west-1.console.aws.amazon.com/config/home?region=eu-west-1#/)
- Add **required-tags** rule
- Set *tag1Key* to **Name**
- You will see that not all resources have got a name tag
![Config Dashboard](../../images/config-dashboard.png)

#### S3 Versioning and Logging
> **Versioning** is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. With versioning, you can easily recover from both unintended user actions and application failures. 

- Learn more about [Versioning](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/enable-versioning.html)

> **Server access logging** provides detailed records for the requests that are made to an S3 bucket. Server access logs are useful for many applications. For example, access log information can be useful in security and access audits. It can also help you learn about your customer base and understand your Amazon S3 bill. 

- Learn more about [Server access logging](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/server-access-logging.html)

**Try It**

- Server access logging is already enabled
- Navigate to S3 by clicking [here](https://s3.console.aws.amazon.com/s3/home?region=eu-west-1#)
- Click on the S3 bucket beginning **lab-**
- Click on the **properties** tab
- Click on the **versioning** box
- Select **Enable versioning**
- Click **Save**





