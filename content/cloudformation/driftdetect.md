---
weight: 4
title: "Drift Detection"
---

AWS CloudFormation allows you to detect if configuration changes were made to your stack resources outside of CloudFormation via the AWS Management Console, CLI, and SDKs. Drift is the difference between the expected configuration values of stack resources defined in CloudFormation templates and the actual configuration values of these resources in the corresponding CloudFormation stacks. This allows you to better manage your CloudFormation stacks and ensure consistency in your resource configurations. For more information on Drift detection, visit the [AWS Blog](https://aws.amazon.com/blogs/aws/new-cloudformation-drift-detection).

In this lab, you will create a CloudFormation stack and purposefully make changes to its resources via other AWS Consoles, so that different types of changes can be visualized using the Drift Detection feature.

**Note - This lab requires you to be in the N.Virginia region**

1. Log in to your AWS account, select the N. Virginia region, and go to the CloudFormation console

2. Click **Create Stack**. 

3. Select **Upload a template to Amazon S3** and upload [templates/my_cfn_stack.yml](../templates/my_cfn_stack.yml)

1. The template defines a Stack with two SQS Queues: an input queue and an error queue.
    
    {{< highlight yaml >}}
    AWSTemplateFormatVersion: "2010-09-09"
    Resources: 
      InputQueue: 
        Type: "AWS::SQS::Queue"
        Properties: 
        QueueName: "DriftLab-InputQueue"
        VisibilityTimeout: 30
        RedrivePolicy: 
          deadLetterTargetArn: 
            Fn::GetAtt: 
              - "DeadLetterQueue"
              - "Arn"
            maxReceiveCount: 5
      DeadLetterQueue: 
        Type: "AWS::SQS::Queue"
        Properties: 
        QueueName: "DriftLab-ErrorQueue"
    {{< /highlight >}}

4. Give the stack a unique name such as drift-lab-with-sqs and **Next**.

5. Leave the options on the next page (parameters, IAM role, etc.) blank and click **Next**.

6. Review the details of your stack and click **Create**.

7. Wait for the stack to complete creation (it might take a few minutes). When complete, the Drift status column will show **NOT_CHECKED**.

8. Select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Detect drift**, then click **Yes** to confirm the drift detection operation.

9. You will see a dialog with the progress of the asynchronous drift operation. When complete, the dialog will inform that the stack is **IN_SYNC**.

    ![](../images/drift_dialog_complete_insync.png)

  > *NOTE: At this point the stack is in the expected Drift state, which is IN_SYNC. You will now simulate a situation that causes the template's definition of resources to diverge from their actual live state, by making changes outside of the CloudFormation Console.*

#### Modifying resources

1. Go to the **Simple Queue Service** Console.

2. Find the DriftLab-InputQueue, select it, and on **Queue Actions**, click **Configure Queue**.

3. Make the following changes to the queue:

    * Set **Default Visibility Timeout** to 50.
    * Set **Delivery Delay** to 120.
    * Uncheck **Use Redrive Policy** to delete the redirection to the error queue on consecutive failures.
    * Click **Save Changes**.
    
    > *NOTE: You have now made a change in the value of an existing property, added a new property, and deleted an existing property of the InputQueue resource.*

4. Go to the CloudFormation Console.

5. Select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Detect drift**, then click **Yes** to confirm the drift detection operation.

6. When complete, this time the dialog result will show the status as **DRIFTED**.

    ![](../images/dift_dialog_complete_drifted.png) 

8. Click on **View details** link on the dialog. 

    > *NOTE: The Drift details are also available from the Stack detail screen. On the Stacks screen, click the drift-lab-with-sqs stack to go to the details screen. As part ofthe stack detail information, notice **Drift status** with a **View details** link.*

9. The **Drift Details** screen shows information of the last drift operation executed for the stack. Notice the **Drift status** is **DRIFTED**.

10. On the **Resource drift status** section, observe the drift status for DeadLetterQueue is **IN_SYNC** since no changes were made. However, the status for InputQueue is **MODIFIED** since the properties specified in the stack's template do not match the live state of the resource.

11. Click to expand InputQueue. On the ride side, under **Differences**, click **Select all**. Note that this will highlight the differences in the template. 

    > *NOTE: The Drift Detection feature is intended to compare differences between the resources and their properties as specified in the template versus their live state. Properties which are not specified in the template will not be shown in the drift status for the resource. In the case of this example, the change to the property Delivery Delay will not be reflected because the property is not specified in the template. The CloudFormation template of a Stack should contain values for all properties which are meant to be compared by Drift Detection.*

#### Remediating by changing resources

One option to recover a stack from a **DRIFTED** state is modifying its resources to revert their properties to the values specified in the stack's template. This will bring the resources inline with the values expected by the template and will allow CloudFormation to make changes based on the actual values during future operations for this stack.

1. Go to the **Simple Queue Service** Console.

2. Find the DriftLab-InputQueue, select it, and on **Queue Actions**, click **Configure Queue**.

3. Make the following changes to the queue:

    1. Set **Default Visibility Timeout** to 30.
    2. Check **Use Redrive Policy**, for **Dead Letter Queue** specify DriftLab-ErrorQueue, and for **Maximum Receives** enter 5.
    3. Click **Save Changes**.

4. Go to the CloudFormation Console.

5. Select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Detect drift**, then click **Yes** to confirm the drift detection operation.

6. When complete, time the dialog result will show the status as **IN_SYNC**.

#### OPTIONAL: Remediating by overwriting property values

  > *Note: applying a stack update operation over resources that are out of sync with their template definition is a RISKY operation. Consider carefully what the differences between the template and the live state of resources represent on each specific scenario and for each specific property. Simulate your exact scenario in a non-production environment. Keep in mind the interaction between resources and the consequences of a stack update rollback (applying the values of the last version of the template). If possible, this operation should be avoided.*

1. Go to the **Simple Queue Service** Console.

2. Find the DriftLab-InputQueue, select it, and on **Queue Actions**, click **Configure Queue**.

3. Make the following changes to the queue:

    1. Set **Default Visibility Timeout** to 80.
    2. Click **Save Changes**.
    
4. Go to the CloudFormation Console.

5. Select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Detect drift**, then click **Yes** to confirm the drift detection operation.

6. When complete, the dialog result will show the status as **DRIFTED**.

7. Edit the template file [templates\my_cfn_stack.yml](../templates/my_cfn_stack.yml)

    1. Set **Default Visibility Timeout** to 40.
    2. Save the file.
    
8. Go to the CloudFormation Console.

9. Select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Update Stack**.

10. Select **Upload a template to Amazon S3** and upload your locally edited template my_cfn_stack.yml

11. Click **Next** on the **Select Template** screen, then click **Next** on the next two pages, and finally click **Update** on the review page.

12. After the stack is in **UPDATE_COMPLETE** state, select drift-lab-with-sqs on the CloudFormation Stacks view, click **Actions** and **Detect drift**, then click **Yes** to confirm the drift detection operation.

13. When complete, the dialog result will show the status as **IN_SYNC**.

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 