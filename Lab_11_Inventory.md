AWS Systems Manager Inventory provides visibility into your Amazon EC2 and on-premises computing environment. You can use Inventory to collect metadata from your managed instances. You can store this metadata in a central Amazon Simple Storage Service (Amazon S3) bucket, and then use built-in tools to query the data and quickly determine which instances are running the software and configurations required by your software policy, and which instances need to be updated. You can configure Inventory on all of your managed instances by using a one-click procedure. You can also configure and view inventory data from multiple AWS Regions and accounts.

In this lab we will enable **Inventory** which leverages **State Manager** to create an **Association** that runs the pre-defined document AWS-GatherSoftwareInventory on an interval that you specify.  We will also configure a **Resource Data Sync** which sends the Inventory data to an S3 bucket for further processing and analysis.  

1.  Navigate to [Systems Manager \> Instances & Nodes \>
    Inventory](https://console.aws.amazon.com/systems-manager/inventory)

2.  Under the Dashboard tab select **Setup Inventory**

3.  Inventory Association Configuration Details

    a.  **Name:** Keep default

    b.  **Targets:** Selecting all managed instances in this account (you
        can target this association if you don't want all instances
        included)

    c.  **Schedule:** Keep default (30 minutes)

    d.  **Parameters:** Keep all defaults

    e.  **Advanced:** Leave unchecked we will configure this separately

4.  Select **Setup Inventory**

5.  Select **View Details** (top right inside the green success banner
    -- If not, go to State Manager and drill into the details of the
    **Association** named **Inventory-Association**

6.  Navigate back to the **Inventory** landing page

    a.  You can now see data is being populated into the Dashboard

![](./media/image22.png)

7.  Select **Detailed View** tab on the **Inventory** landing page

8.  Make an S3 bucket before creating the **Resource Data Sync**

    a.  **NOTE:** If you did the Distributor Lab and created a bucket
        for **Systems Manager** usage then just use that and create an
        Inventory prefix so your data remains organized

    b.  Navigate to [S3](https://s3.console.aws.amazon.com/s3)

    c.  Select **Create Bucket**

    d.  Enter: YOURFIRSTNAME10-sm

    e.  **Region:** us-east (N. Virginia)

    f.  Keep all defaults

    g.  Block all public access

    h.  Create Bucket

    i.  Go to the **Permissions** tab

    j.  Go to Bucket policy

    k.  Enter in your Bucket Name and select save

```
{

    "Version": "2012-10-17",

    "Statement": [

        {

            "Sid": "SSMBucketPermissionsCheck",

            "Effect": "Allow",

            "Principal": {

                "Service": "ssm.amazonaws.com"

            },

            "Action": "s3:GetBucketAcl",

            "Resource": "arn:aws:s3:::ENTERYOURBUCKET"

        },

        {

            "Sid": " SSMBucketDelivery",

            "Effect": "Allow",

            "Principal": {

                "Service": "ssm.amazonaws.com"

            },

            "Action": "s3:PutObject",

            "Resource": [

                "arn:aws:s3:::ENTERYOURBUCKET/inventory/*"

            ],

            "Condition": {

                "StringEquals": {

                    "s3:x-amz-acl": "bucket-owner-full-control"

                }

            }

        }

    ]

}
```

9.  Navigate back to [Inventory \> Resource Data
    Sync](https://console.aws.amazon.com/systems-manager/managed-instances/resource-data-sync)

10. Select Create resource data sync

11. Configuration details

    a.  **Sync name:** YOURNAME-inventory-s3-sync

    b.  **Bucket name:** the name of the bucket your created previously

    c.  **Bucket prefix:** inventory

    d.  **Bucket region:** us-east-1

    e.  **KMS key:** leave blank for the purpose of the demo

    f.  Select **Create**

12. Switch back to your bucket and you can now see the data being synced

    a.  ![](./media/image23.png)

    b.  Now we have a clean data structure for inventory data

    c.  From here you can utilize Athena and Quicksight to gain deeper
        insight about the inventory data gathered

    d.  **Tutorial:**
        <https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-inventory-datasync.html>

![](./media/image24.png)