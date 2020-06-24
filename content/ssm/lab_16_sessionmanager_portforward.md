---
title: "Session Manager - Port Forwarding"
date: 2020-06-T14:10:29-04:00
weight: 18
chapter: false
pre: "<b>18. </b>"
---

One common thing that is mentioned when showing **Session Manager** to
folks new to **Systems Manager** is that it doesn't address RDP
sessions. Port Forwarding utilizes SSH tunneling to establish a secure
tunnel between localhost and a remote service.

![](./media/image39.png)

This command tells SSH to connect to instance as user ec2-user, open
**port 9999** on my local laptop, and forward everything from there
to **localhost:80** on the instance. When the tunnel is established, I
can point my browser at **http://localhost:9999** to connect to my
private web server on **port 80.** This lab will demonstrate how to
connect to a Windows instance via Remote Desktop Protocol via **Port
Forwarding with Session Manager**.

### Deploy Windows Instance

1.  Navigate to the [EC2 Console](https://console.aws.amazon.com/ec2)

2.  Go to Instances

3.  Launch Instance

    a.  Microsoft Windows Server 2019 Base

    b.  t2.small

    c.  Configure instance details

    d.  Number of instances: 1

    e.  Default VPC

    f.  No preference on subnet

    g.  Ensure auto-assign public IP is enabled

    h.  IAM Role: ManagedInstancesRole (previously created -- this is
        what allows instances to work with Systems Manager)

    i.  Default Storage

    j.  Leave Tags as is -- We will create some later in the workshop

    k.  Create a new Security Group -- Allow TCP 3389 from anywhere

    l.  Launch

    m.  Select Key Pair that you previously created (this will be used
        to retrieve the admin password for the instance)

4.  Go back to view instances and ensure that all 1 transition to an
    Instance State of running

5.  Navigate to [Systems Manager \> Instances & Nodes \> Managed
    Instances](https://console.aws.amazon.com/systems-manager/managed-instances)

6.  Ensure that the new instance is listed (if not check your IAM role
    attached to the instance)

7.  Grab the instance ID as you will need this for the next section

### Establish CLI Session

1.  Open an AWS CLI session (refer to Accessing AWS Account for your AWS
    keys)

2.  Run the following command

    a.  *aws ssm start-session \--target \"Your Instance ID\"
        \--document-name AWS-StartPortForwardingSession \--parameters
        \"portNumber\"=\[\"3389\"\],\"localPortNumber\"=\[\"56789\"\]*

3.  You will see that your session has started

![](./media/image40.png)

4.  Navigate to [Systems Manager \> Instances & Nodes \> Session
    Manager](https://console.aws.amazon.com/systems-manager/session-manager/sessions)

5.  You will now see your session open inside the console

![](./media/image41.png)

6.  Navigate back EC2 and get your password

    a.  Select **Connect** on your instance

    b.  Select **Get password**

    c.  Select your Key Pair used to deploy the instance

7.  Open your RDP client

8.  Connect to localhost:56789

![](./media/image42.png)

9.  Log in with your Administrator user and password

    a.  If you switch back to your console you will see the latest
        message is "Connection accepted for session -- Meaning you
        established a connection with the remote service on the defined
        local port in your command

10. Wait for login -- t2.micro (t2.small is what should have been used)
    is a bit undersized for Windows Server 2019 and took a while to log
    in

11. Once logged in you can query the instance meta-data to verify you
    are on the right instance -- Open Powershell console

    a.  **Invoke-RestMethod -uri
        http://169.254.169.254/latest/meta-data/public-hostname**

    b.  **Invoke-RestMethod -uri
        http://169.254.169.254/latest/meta-data/instance-id**

12. Got back to your AWS CLI session and kill the command -- You will
    see the RDP session terminate as the tunnel is torn down

13. Navigate back to Session Manager \> Session History and you can see

    a.  Session owner ARN

    b.  Instance ID that was connected to

    c.  Start and end date and time of session