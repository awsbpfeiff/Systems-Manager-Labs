AWS Systems Manager Distributor lets you package your own software—and find AWS-provided agent software packages, such as AmazonCloudWatchAgent, or third-party packages such as Trend Micro—to install on AWS Systems Manager managed instances. Distributor publishes resources, such as software packages, to AWS Systems Manager managed instances. Publishing a package advertises specific versions of the package's document—a Systems Manager document that you create when you add the package in Distributor—to managed instances that you identify by managed instance IDs, AWS account IDs, tags, or an AWS Region.

After you create a package in Distributor, which creates an AWS Systems Manager document, you can install the package in one of the following ways:

* One time by using **AWS Systems Manager Run Command**

* On a schedule by using **AWS Systems Manager State Manager**

[Reference Video](https://www.youtube.com/watch?v=AvQWkfgEQI8)

In this lab we will create a custom package that deploys the AWS Kinesis Agent.  Additional configuration is required to utilize the agent which can be found [here](https://docs.aws.amazon.com/firehose/latest/dev/writing-with-agents.html#download-install) but is out of scope for this lab.

1.  You will need to make an S3 bucket to store the package in:

    - Navigate to [S3](https://s3.console.aws.amazon.com/s3)

    - Select **Create Bucket**

    - For **Bucket Name** Enter: **YOURFIRSTNAME10-sm-distributor**

    - For **Region** ensure ```us-east-1``` is selected

    - For **Bucket settings for Block Public Access** ensure the check box is selected

    - **Keep all defaults** for the remaining items

    - Select **Create Bucket**

1.  Download the following package locally:

    [AWS Kinesis Agent](https://s3.amazonaws.com/streaming-data-agent/aws-kinesis-agent-latest.amzn2.noarch.rpm)

1.  Navigate to [Systems Manager \> Instances & Nodes \>
    Distributor](https://console.aws.amazon.com/systems-manager/distributor)

1.  Select **Create Package**

1.  Select **Simple Package** (Advanced allows you to specify your own
    install/uninstall scripts)

1.  Enter: **Kinesis-Agent** for the name

1.  Select the bucket you made in the previous step

1.  Enter a prefix of **Kinesis-Agent**

1.  Select **Add Software** under Upload

    - Select the **Kinesis rpm** you downloaded in step 2

    - Set the **Target Platform** as ```amazon```

    - Set Platform Version as ```_any```

    - Set **Architecture** as ```_any```

1. If you expand **Scripts** you can see that distributor has already
        provided the appropriate installation / uninstallation scripts

    ![](./media/distributor-upload-software.png)

1. If you expand **Manifest** you will see the package you are
    installing and the which package manager to use depending on the selected Operating Systems

    ![](./media/distributor-manifest.png)

1. Select **Create Package**

1. Your manifest file and package data will be uploaded to the
    specified S3 bucket

    ![](./media/distributor-s3.png)

### Install Custom Package

Now that you have your custom package uploaded to your S3 bucket along
with the manifest. Distributor gives you 2 quick options to deploy your
package. You can either install on a schedule or install one time.
Installing on a schedule automatically prepares a **State Manager
Association** with the pre-defined **Document** of
**AWS-ConfigureAWSPackage** and the name of your custom package as a
parameter. Install one time does the same preparation but uses **Run Command**.

1. Navigate to [Systems Manager \> Instances & Nodes \>
    Distributor](https://console.aws.amazon.com/systems-manager/distributor)

1. Choose the **Owned by me** tab

1. Select the Radio button next to **Kinesis Agent**

1. Choose **Install one time**

    ![](./media/distributor-install-one-time.png)

1. This will pre-populate the Run Command with all of the necessary configuration items to execute against the instances you choose

    - **NOTE:** Run Command uses the Amazon Managed command document - AWS-ConfigureAWSPackage to execute the package you created
    ![](./media/distributor-execute-package.png)

1. For **Targets** select **Choose instances manually**

1. Select **App1** and **App2** as the instances that will get the new **Distributor** package applied

1. Leave the remaining configuration details as default

2. **EXCEPT:** Under **Output Options** leave the check for **Enable writing to an S3 bucket**

    - Chose the bucket you created earlier from the drop down and enter **Kinesis-Agent-logs** as the prefix

    ![](./media/distributor-s3-log.png)

3. Choose **Run**

    ![](./media/distributor-run.png)

4. You are now redirected to the to the **Run Command** progress for the distributor package deployment

    ![](./media/distributor-run-command-progress.png)

5. Hit the refresh button:

    ![](media/distributor-refresh.png)

6. The command should have completed successfully.  Under **Targets and Outputs** Highlight the radio button next to instance ID and select **View Output**

    ![](./media/distributor-view-output.png)

7. Click on the **Amazon S3** link to bring you over to the full log output.  The **Run Command** console will only show 2500 characters of the log output. 

    ![](./media/distributor-s3-output.png)

1. Once in the S3 bucket choose the folder **configurePackage**

1. Select the file **stdout**

1. Choose the **Select from** tab

1. Select **Show file preview** select **Next**

1. Under **SQL Expression** in **SQL Editor** change the limit from 5 to 300

1. Choose **Run SQL**

1. Scroll to the bottom of the log and you will see the successfully installed message!

    ![](./media/distributor-s3-sql-complete.png)

1. You have now configured a **Distributor package** with custom software, used **Run Command** to deploy the package to a **Managed Instance**, and reviewed the Log output in **S3**.  The logs can also be sent to **CloudWatch Logs**. 



