AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances. A managed instance is any EC2 instance or on-premises machine in your hybrid environment that has been configured for Systems Manager. Run Command enables you to automate common administrative tasks and perform ad hoc configuration changes at scale. You can use Run Command from the AWS console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs. Run Command is offered at no additional cost.

Administrators use Run Command to perform the following types of tasks on their managed instances: install or bootstrap applications, build a deployment pipeline, capture log files when an instance is terminated from an Auto Scaling group, and join instances to a Windows domain, to name a few.

This section we will utilize the Run command to execute our newly built
Document against our application instances.

1.  Navigate to the [Systems Manager Console \> Instances and Nodes \>
    Run
    Command](https://console.aws.amazon.com/systems-manager/run-command)

2.  Select **Run Command**

3.  Select inside the search box under **Command Document** to apply a
    filter

    a.  Select or enter -- Owner : Owned by me

    b.  Select **org-install-app**

4.  Under Document Version select **Latest Version at Runtime**

5.  Under **Targets** we will select a **Resource Group**

    a.  Select Web (*created earlier in Resource Groups + Tags section*)

    b.  Leave **resource types** as All available resource types

6.  Leave other parameters and Rate Control as default

7.  Uncheck enable writing to an S3 bucket

8.  Uncheck enable writing to CloudWatch Logs ([ideal solution to
    storing and viewing
    logs](a.%09https:/docs.aws.amazon.com/systems-manager/latest/userguide/sysman-rc-setting-up-cwlogs.html))
    -- (*Unchecked by default*)

9.  Select **Run**

10. You will be brought over to the Command Status of the Run Command
    session you created

    ![](./media/image4.png)

11. Select one the instance IDs to drill down for details about the
    execution of the run command (stdout)

    a.  Expand **Step 1 -- Output** (Max of 2500 characters)

    b.  Alternatively select CloudWatch Logs and you can view the entire
        output

12. We will now use Session Manager to view that our Run command was
    successful
