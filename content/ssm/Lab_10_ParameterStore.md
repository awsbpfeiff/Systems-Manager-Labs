Parameter Store is a secure location to keep secrets and configuration
information. It allows you to dynamically and securely retrieve data as
you need it vs saving it within the Operating System or configuration
files.

1.  Navigate to [Systems Manager \> Application Management \> Parameter
    Store](https://console.aws.amazon.com/systems-manager/parameters)

2. Fill out the data for adding your secret

3. **Name:** YOURNAME-secret1

4. **Description:** blank

5. **Tier:** Standard

6. **Type:** SecureString

7. **KMS Key Source:** My current account (uses the default KMS
    key or specify a CMK of your choice)

8. **KMS Key ID:** alias/aws/ssm (AWS managed key for Systems
    Manager)

9. **Value:** This is your secret data whether that is configuration
    data, passwords, connection strings, etc...

10. **Tags:** your choice -- this is ideal to organize your secrets so
    you do not get lost --

    a.  Key: Team / Value: Operations

    b.  Key: Application / Value: RevenueGen1

    c.  Key: Owner / YOURNAME

11. Select **Create Parameter**

12. You are brought back to the **Parameter Store** home screen and now
    select your new secret

13. You can Select **Show** to reveal the contents of the secret

![](./media/image17.png)

14. History will show you the users who created, updated, or deleted the
    secret

15. Versions of the parameter are kept but if you delete the parameter
    then the history is deleted as well

### Retrieve Secret from CLI \[Optional\]

Most use cases you would not be using the Management Console to retrieve
your secrets. You would be using the CLI or SDK to programmatically
gather this information as part of the task you are performing. Below is
a basic exercise to gather the secret you made previously.

1.  Navigate back to <https://dashboard.eventengine.run>

2. Pull up the credentials from the Event Engine Dashboard

![](./media/image18.png) 

Select **AWS Console**

![](./media/image2.png)

Gather your access keys

3. Install AWS CLI -
    <https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html>

4. Once installed run aws configure and use the access keys above --
    region: us-east-1

5. We made our secret a SecureString -- When we run aws ssm
    get-parameter without the decryption flag you can see the value of
    the parameter is obscured

6. Command: aws ssm get-parameter \--name "YOURNAME-secret1"

 ![](./media/image19.png)

7. Now we add the with decryption flag

8. Command: aws ssm get-parameter \--name \"bills-secret1\"
    \--with-decryption

![](./media/image20.png)

9. You can see that the value is now in plain text

10. Then you would parse the JSON output with something like jq to be
    able to get the raw value

11. Command: aws ssm get-parameter \--name \"bills-secret1\"
    \--with-decryption \| jq -r \".Parameter.Value\"

![](./media/image21.png)
