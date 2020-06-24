---
weight: 3
title: "Comformance Packs"
---

#### Deploy Components for Lab

In this course, we will be deploying Amazon S3 Operational Best Practices with remediation actions conformance pack in an AWS Account. This pack contains following AWS Config Rules.

* S3BucketPublicReadProhibited with Remediation Action
* S3BucketPublicWriteProhibited with Remediation Action
* S3BucketReplicationEnabled
* S3BucketSSLRequestsOnly
* S3BucketServerSideEncryptionEnabled with Remediation Action
* S3BucketLoggingEnabled with Remediation Action

#### Prerequisites

We will create prerequisite resources required for “Amazon S3 Operational Best Practices with Remediation Actions” conformance pack. This includes service linked role for Conformance Packs, Remediation action automation assume role and S3 Service Side logging bucket.

1. Login to your AWS Account.
2. Select the appropriate Region for your Event Engine event. You can select any region where AWS Config conformance packs are available for this excercise. Click [here](https://aws.amazon.com/about-aws/whats-new/2019/11/introducing-aws-config-conformance-packs/) to see list of regions.
3. Go to the CloudFormation Console, and click Create stack (with new resources).
4. Under Prerequisite - prepare template, select Template is ready.
5. Download the template from [here](https://reinvent2019.aws-management.tools/mgt405/templates/prerequisite-resources.yaml) to your machine.
6. Under Specify template, select Upload a template file and click Choose file button and select CloudFormation template downloaded from the previous step.
7. Enter Stack name. Keep the setting with the default value and click Next.
8. Leave all defaults on the Configure stack options page and click Next.
9. On the Review Test step, in the Capabilities section, check the check box I acknowledge that AWS CloudFormation might create IAM resources with custom names. and complete the Stack creation.


#### Enable AWS Config

We will enable AWS Config to begin change tracking of your resources.  If you previously enabled Config, please skip this section.

1. Go to the AWS Config Console. If this is your first time using AWS Config, select Get started. If you’ve already used AWS Config, select Settings.
2. In the Settings page, under Resource types to record, select Record all resources supported in this region checkbox. Also check the checkbox for global resources.
3. Under Amazon S3 bucket, select Create a bucket.
4. Optional - Under Amazon SNS topic, check the box for Stream configuration changes and notifications to an Amazon SNS topic, and then select the radial button, Create a topic.
5. Under AWS Config role, choose Create AWS Config service-linked role. or Use an existing AWS Config serivce-linked role (unless you already have a role you want to use).
6. If this is the first time using AWS Config, Click Next to Create Rule. Click Skip to go to Review page. Click Confirm.


#### Deploy Conformance Pack

We will deploy **Amazon S3 Operational Best Practices** with remediation conformance pack.

1. Go to the [Config Console](https://console.aws.amazon.com/config). Click here to navigate to new console. Conformance Packs are available in new redesigned AWS Config console only.
2. Click on Conformance Packs from left navigation panel.
3. Click on Deploy conformance pack on the top right of the page.
4. Under template details, select Template is ready.
5. Download template from [here](https://docs.aws.amazon.com/config/latest/developerguide/templateswithremediation.html). Copy the contents of the **Operational Best Practices For Amazon S3 with Remediation** comformance pack into a text editor and replace “Account ID” on line 43, 80, 139 and 179 with your accountId.  Save the contents of the text editor into a file named  S3ConformancePack.yml.
6. Under template location, select Upload a template file and click Choose file button and select template downloaded in previous step.
7. Enter Conformance Pack Name.
8. Enter S3 Bucket Name as awsconfigconforms-delivery-bucket-{YOUR-AWS-ACCOUNT-ID}. This bucket was created as part of prerequisite cloudformation template in previous section.  If you used a different bucket name, use the appropriate name.
9. **IMPORTANT:** Under parameter, click on Add Parameter. Operational Best Practices for Amazon S3 conformance pack require a template parameter with name S3TargetBucketNameForEnableLogging.
10. Enter Parameter Key as S3TargetBucketNameForEnableLogging and Parameter Value as s3serversideloggingbucket-{YOUR-AWS-ACCOUNT-ID} and Click Next.
11. Click Deploy Conformance Pack to deploy

#### View Confliance Remediation

We will check compliance status for each rule in conformance pack and associated resources.  Conformance Packs can also be deployed to an AWS Organizations; however, in the interest of time it will only be discussed during thiis lab.

1. Once conformance pack is deployed, Click on conformance pack name to drill down into details. You can view list of rules and their compliance status.
2. Click on a rule name to see rule details.
3. Expand Resources in Scope section to see resources in scope and their compliance status. If there are any existing non-compliant resources, you can manually remediate them or wait for auto-remediation to kick in.
4. To see auto-remediation in action on a new resource, create a new s3 bucket using S3 Console. AWS Config will discover the resource and mark it as non-compliant if it is not following S3 Best Practices.
5. Go back to Conformance Pack details and select a rule with remediation action.
6. Expand Resources in Scope section to see newly created resource with its compliance status. If the resource is non-compliant, auto-remediation action will apply to resource within few minutes.
7. Refresh the page to see updated resource compliance status.
8. You can also manually remediate a resource by selecting resource and clicking Remediate

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 