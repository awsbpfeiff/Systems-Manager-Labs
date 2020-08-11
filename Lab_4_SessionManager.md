In this lab we will utilize **Session Manager** to connect to our App
instances that we executed our Run command against. Session Manager is a
tool that allows administrators Quickly and securely access your Amazon
EC2 instances through an interactive one-click browser-based shell or
through the AWS CLI without the need to open inbound ports, maintain
bastion hosts, or manage SSH keys.

The IAM user or role must have Session Manager permissions as well as
access to the target Managed Instances. When a version of SSM Agent that
supports Session Manager starts on an instance, it creates a user
account with root or administrator privileges called **ssm-user**. On
Linux machines, the account is added to **/etc/sudoers**. On Windows
machines, it is added to the **Administrators** group. Sessions are
launched using these user accounts. On Windows machines you will be
dropped into a Powershell terminal .

1.  Navigate to [Systems Manager \> Instances & Nodes \> Session
    Manager](https://console.aws.amazon.com/systems-manager/session-manager)

2.  Select **Start Session**

3.  Select one of the **Web** servers we were just working on

4.  Select **Start Session**

5.  You will be directed to a new tab and presented with the shell of
    the target **Web** server

6.  Switch back to the **Session Manager** tab and select refresh -- You
    can now see the active session you've established

7.  Switch back to the tab with your active session and type following
    commands

    a.  pwd - output should be **/usr/bin**

    b.  cd /app

    c.  ls

        i.  You can see our text file "hello.txt" created by the Command Document we executed with Run Command earlier

    d.  systemctl status httpd -- Apache is now running

8.  Select **Terminate** at the top right of the session

9.  Go back to **Session Manager** and select refresh -- You can see the
    session was ended and there are no running sessions

10. Select **Session History**

11. You will see your previous session in a Terminating state -- We will
    now configure session logging in CloudWatch Logs

12. Navigate to [Services \> Management & Governance \> CloudWatch \>
    Log Groups](https://console.aws.amazon.com/cloudwatch)

13. Select Actions \> Create Log Group

    **NOTE:** Your role needs to have the [appropriate permissions
        within CloudWatch](https:/docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging-auditing.html#session-manager-logging-auditing-cloudwatch-logs)

    In this lab, the team role has the necessary permissions.  

14. Enter **Systems-Manager-Workshop** as the name

15. Navigate back to Session Manager

16. Configure **Preferences**

    a.  ![](./media/image5.png)

17. Under Send Session output to CloudWatch Logs check the CloudWatch
    logs box

    a.  Uncheck Encrypt log data (can be enabled on the CloudWatch (CW)
        Logs group)

    b.  Select a log group name from the list =
        **Systems-Manager-Workshop**

    c.  Select **Save**

18. Repeat steps 4-8

19. Return to **Session Manager \> Session History** (This will be in a
    terminating state for 1-3 minutes as the history is sent to
    CloudWatch Logs for storage)

    a.  Select refresh

20. Once the status is **Terminated** you can select the output location
    -- **CloudWatch Logs** (open in a new tab since it will redirect the
    page that you're are currently on)

21. In CloudWatch Logs you will see that inside your Systems Manager Log
    Group a new Log Stream was created and utilized the ID of the
    Session Manager session you just ended

22. Expand the messages and you will see a full output of the terminal
    session

    a.  ![](./media/image6.png)

23. This is powerful because now you have an easy interface to review
    session logs and you can also create metric filters and alert of
    specific log file entries (e.g. sudo) and send messages to an SNS
    topic and the communications type
