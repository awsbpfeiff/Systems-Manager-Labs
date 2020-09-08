---
title: "Maintenance Windows"
date: 2020-06-T14:10:29-04:00
weight: 10
chapter: false
pre: "<b>10. </b>"
---

AWS Systems Manager Maintenance Windows let you define a schedule for when to perform potentially disruptive actions on your instances such as patching an operating system, updating drivers, or installing software or patches. Maintenance Windows also lets you schedule actions on numerous other AWS resource types, such as Amazon Simple Storage Service (Amazon S3) buckets, Amazon Simple Queue Service (Amazon SQS) queues, AWS Key Management Service (AWS KMS) keys, and many more. For a full list of supported resource types that you can include in a maintenance window target, see [Supported Resources for AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/supported-resources.html#supported-resources-console) in the AWS Resource Groups User Guide.

Each maintenance window has a schedule, a maximum duration, a set of registered targets (the instances or other AWS resources that are acted upon), and a set of registered tasks. You can add tags to your maintenance windows when you create or update them. (Tags are keys that help identify and sort your resources within your organization.) You can also specify dates that a maintenance window should not run before or after, and you can specify the international time zone on which to base the maintenance window schedule.

In this lab we will create a Maintenance Window to be used within Patch Manager. We will be using the default service-linked role for Systems Manager. Custom Roles can be created to restrict actions a service can perform within a Maintenance Window and selected during the association of a Maintenance Window and a service.

### High-level Objectives

*  Create the window and define its schedule and duration.
*  Assign targets for the window.
*  Assign tasks to run during the window

### Create Maintenance Window

First, you must [create the
window](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-create-mw.html) and
define its schedule and duration:

1.  Open the AWS [Systems Manager
    console](https://console.aws.amazon.com/systems-manager/).

1.  In the navigation pane, select **Maintenance Windows** and then
    select **Create a Maintenance Window**.

1.  In the **Provide maintenance window details** section:
    - In the **Name** field, type a descriptive name to help you
        identify this Maintenance Window, such
        as ```Patch-amznlin2-app```.
    - (Optional) you may enter a description in
        the **Description** field.
    - Select **Allow unregistered targets** if you want to allow a
        Maintenance Window task to run on managed instances, even if you
        have not registered those instances as targets.
    - **Note:** If you select **Allow unregistered targets**, then you
        can select the unregistered instances (by instance ID) when you
        register a task with the Maintenance Window. If you don't, then
        you must select previously registered targets when you register
        a task with the Maintenance Window.
        - Specify a schedule for the Maintenance Window by using one
            of the scheduling options:
        - To have the maintenance window execute more rapidly while
            engaged with the lab:
            - Select **Rate schedule builder** under Schedule
            - Set **Window starts** to 5 minutes.
        - Real world configuration would be similar to:
            - Under **Specify with**, use **Cron schedule builder**.
            - Under **Window starts**, select the third option,
                specify **Everyday**, and select a time, such as 02:00.
            - In the **Duration** field, type the number of hours the
                Maintenance Window should run, such as ```2``` **hours**.
            - In the **Stop initiating tasks** field, type the number
                of hours before the end of the Maintenance Window that
                the system should stop scheduling new tasks to run, such
                as 1 **hour before the window closes**. Allow enough
                time for initiate activities to complete before the
                close of the maintenance window.

1.  Select **Create maintenance window**. The system returns you to
    the **Maintenance Window** page. The state of the Maintenance Window
    you just created is **Enabled**.

### Assigning Targets to the Patch Window

After you create a Maintenance Window, you [assign
targets](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-assign-targets.html) that
the tasks will run against. In this case, we will utilize the Patch
Group we created previously containing the 2 app servers.

1.  On the **Maintenance windows** page, select the **Window ID** of
    your maintenance window to enter its Details page.

1.  Select **Actions** in the top right of the window and
    select **Register targets**.

1.  On the **Register target** page under **Maintenance window target
    details**:
    - In the **Target Name** field, enter a name for the targets, such
        as ```App```.
    - (Optional) Enter a description in the **Description** field.
    - (Optional) Specify a name or work alias in the **Owner
        information** field. **Note**: Owner information is included in
        any CloudWatch Events that are raised while running tasks for
        these targets in this Maintenance Window.

1.  In the **Targets** section, under **Select Targets by**:
    - Select the default **Specifying tags** to target instances by
        using Amazon EC2 tags that were previously assigned to the
        instances.
    - Under **Tags**, enter ```Patch Group``` as the key and ```App``` as the
        value. The option to add an additional tag key/value pair will
        appear.

1.  Select **Register target** at the bottom of the page to return to
    the maintenance window details page.

If you want to assign more targets to this window, select
the **Targets** tab, and then select **Register target** to register new
targets. With this option, you can select a different means of
targeting. For example, if you previously targeted instances by instance
ID, you can register new targets and target instances by specifying
Amazon EC2 tags.

### Assigning Tasks to Maintenance Window

This part is critical to ensure that the IAM role is setup properly for
Maintenance Window use. After you assign targets, you [assign
tasks](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-assign-tasks.html) to
perform during the window:

1.  From the details page of your maintenance window,
    select **Actions** in the top right of the window and
    select **Register Run command task**.

1.  On the **Register Run command task** page:
    - In the **Name** field, enter a name for the task, such
        as ```PatchTestAppServers```.
    - (Optional) Enter a description in the **Description** field.

1.  In the **Command document** section:
    - Select the search icon, select Platform Types, and then
        select Linux to display all the available commands that can be
        applied to Linux instances.
    - Select **AWS-RunPatchBaseline** in the list.
    - Leave the **Task priority** at the default value of **1** (1 is
        the highest priority).
    - Tasks in a Maintenance Window are scheduled in priority order,
        with tasks that have the same priority scheduled in parallel.

1.  In the **Targets** section:
    - For **Target by**, select **Selecting registered target
        groups**.
    - Select the group you created from the list.

1.  In the **Rate control** section:
    - For **Concurrency**, leave the default **targets** selected and
        specify 1.
    - For **Error threshold**, leave the default **errors** selected
        and specify 1.

1.  In the **Role** section, specify the role you defined with the
    AmazonSSMMaintenanceWindowRole. It will
    be **SM-Workshop-MaintenanceWindowRole** if you followed the
    suggestion in the instructions above.

1.  In **Output options**, leave **Enable writing to S3** unchecked.
    - (Optionally) Specify **Output options** to record the entire
        output to a preconfigured **S3 bucket** and optional **S3 key
        prefix** \>**Note**\
        Only the last 2500 characters of a command document's output are
        displayed in the console. To capture the complete output define
        and S3 bucket to receive the logs.

1.  In **SNS notifications**, leave **Enable SNS
    notifications** unchecked.
    - (Optional) Specify **SNS notifications** to a
        preconfigured **SNS Topic** on all events or a specific event
        type for either the entire command or on a per-instance basis.

1.  In the **Parameters** section, under **Operation**,
    select **Install**.

1. You can also toggle if you want a Reboot to be triggered after
    installation or schedule it at another time (NoReboot)

1. Select **Register Run command task** to complete the task definition
    and return to the details page.

### Reviewing Run Command Status and Output

This section will show you how to check the status of the Run command
using the Management Console.

1.  After allowing enough time for your maintenance window to complete:
    - Navigate to the AWS [Systems Manager
        console](https://console.aws.amazon.com/systems-manager/).
    - Select **Maintenance Windows**, and then select the **Window
        ID** for your new maintenance window.

1.  On the **Maintenance window ID** details page, select **History**.

1.  Select a **Windows execution ID** and select **View details**.

1.  On the **Command ID** details page, scroll down to the **Targets and
    outputs** section, select an **Instance ID**, and select **View
    output**.

1.  Select **Step 1 - Output** and review the output.

1.  Select **Step 2 - Output** and review the output.

### Checking Patch Compliance

This section will cover

1.  Under **Instances & Nodes** in the AWS Systems Manager navigation
    bar, select **Compliance**.

1.  On the **Compliance** page in the **Compliance resources summary**,
    you will now see that there are 2 systems that are listed as
    **Compliant Resources**. In the **Resources** list, you will see the
    individual compliance status and details.

1.  You can also sort by **Patch Group** or **Resource Group**
