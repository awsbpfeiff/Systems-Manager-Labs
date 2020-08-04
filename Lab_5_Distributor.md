Distributor is a capability that allows you to create custom packages
for deployment to your Managed Instances. This could be helpful for
application or tools deployment to your fleet. In this scenario we will
deploy Powershell Core to a Linux Managed Instance.

**Reference Video =** <https://www.youtube.com/watch?v=AvQWkfgEQI8>

1.  Need to make an S3 bucket to store the package in

    a.  Navigate to [S3](https://s3.console.aws.amazon.com/s3)

    b.  Select Create Bucket

    c.  Enter: YOURFIRSTNAME10-sm

    d.  Region: us-east (N. Virginia)

    e.  Keep all defaults

    f.  Block all public access

    g.  Create Bucket

2.  Download the following package locally:

    a.  <https://github.com/PowerShell/PowerShell/releases/download/v6.2.4/powershell-6.2.4-1.rhel.7.x86_64.rpm>

3.  Navigate to [Systems Manager \> Instances & Nodes \>
    Distributor](https://console.aws.amazon.com/systems-manager/distributor)

4.  Select **Create Package**

5.  Select **Simple Package** (Advanced allows you to specify your own
    install/uninstall scripts)

6.  Enter: **PowerShell-linux** for the name

7.  Select the bucket you made in step 1

8.  Enter a prefix of Linux

9.  Select **Add Software** under Upload

    a.  Select the rpm you downloaded in step 2

    b.  Set the **Target Platform** as amazon

    c.  Set Platform Version as \_any

    d.  Set **Architecture** as x86\_64

    e.  If you expanded scripts you can see that distributor has already
        provided the appropriate install / uninstallation scripts

    f.  ![](./media/image7.png)

10. If you expand **Manifest** you will see the package you are
    installing and the which installers to use depending on OS

    a.  ![](./media/image8.png)

11. Select **Create Package**

12. Your manifest file and package data will be uploaded to the
    specified S3 bucket

### Install Custom Package

Now that you have your custom package uploaded to your S3 bucket along
with the manifest. Distributor gives you 2 quick options to deploy your
package. You can either install on a schedule or install one time.
Installing on a schedule automatically prepares a **State Manager
Association** with the pre-defined **Document** of
**AWS-ConfigureAWSPackage** and the name of your custom package as a
parameter. Install one time does the same preparation but uses **Run
Command**.

![](./media/image9.png)
