OpsCenters helps provide a regional dashboard to surface infrastructure
related issues. OpsCenter can report on any data that can be fed into
CloudWatch Event Rules and configured as an SSM OpsItem. In this example
we will enable OpsCenter and review the default rules in creates and
look at what a live issue looks like in the dashboard.

1.  Navigate to [Systems Manager \> Operations Management \>
    OpsCenter](https://console.aws.amazon.com/systems-manager/opsitems)

2.  Select **Get started**

3.  Leave the Default Rules item checked

4.  Select **Enable OpsCenter**

    a.  ![](./media/image27.tiff){width="6.330768810148731in"
        height="1.4309864391951006in"}

5.  Several default Roles are used to automatically create several
    default SSM OpsItems Rules in CloudWatch Events

    a.  ![](./media/image28.png){width="5.830768810148731in"
        height="2.379870953630796in"}

    b.  Permissions -
        <https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-setup-permissions.html>

6.  Let's create an OpsItem / Rule so you can see the power of OpsCenter
    and its ability to aggregate data across AWS services

    a.  Navigate to [CloudWatch \> Events \>
        Rules](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#rules:)

    b.  Select **Create Rule**

    c.  Event Source = Event Pattern

    d.  Service Name = EC2

    e.  Event Type = EC2 Instance State-change Notification

    f.  Specific States = Terminated

    g.  Any instance

    h.  Target = SSM opsItem

    i.  Role = Allow create new role

    j.  Select **Configure Details**

    k.  Name = YOURNAME-test-ec2-terminated

    l.  State = Enabled

    m.  Select **Create Rule**

    n.  ![](./media/image29.png){width="6.461538713910761in"
        height="3.524647856517935in"}

7.  Navigate Back to [Systems Manager \> Operations Management \>
    OpsCenter \> OpsItems \> Configure
    Sources](https://console.aws.amazon.com/systems-manager/explorer/settings?region=us-east-1&source=opscenter)

    a.  You will now see the new CW rule we created

    b.  ![](./media/image30.png){width="3.884615048118985in"
        height="1.953325678040245in"}

8.  Navigate to
    [EC2](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:sort=tag:Name)

9.  Select an instance and terminate it

    a.  Highlight instance \> Actions \> Instance State \> Terminate

10. Navigate to
    [OpsCenter](https://console.aws.amazon.com/systems-manager/opsitems/?region=us-east-1#activeTab=REPORTING)

11. You will now see OpsItems by Source and age -- Grouped by source =
    EC2

    a.  Under count select the number or go to OpsItem status summary
        and select one of the open items

    b.  ![](./media/image31.png){width="5.292307524059493in"
        height="1.2014326334208223in"}

    c.  You will now see our event -- Select the ID (e.g.
        oi-\#\#\#\#\#\#\#\#\#\#\#\#)

    d.  You will see a ton of data regarding the event (aggregate
        sources)

        i.  Related resources shows the ARN of the instance your
            terminated

        ii. Runbooks is powerful as it allows you to take action of the
            event via runbook as the event occurs

        iii. Operational Data will show you the instance ID, the state,
             and the ClouwdWatch Event that generated it

    e.  Go back to **Related resource details** (top left)

        i.  ![](./media/image32.png){width="4.3769225721784775in"
            height="1.6691721347331583in"}

        ii. **Resource description** shows you an output of the meta
            data that is displayed in the EC2 console when the instance
            is running

        iii. Tags for the instance are shown

        iv. Details from Config (might take some time to show up in
            Opsitems)

        v.  CloudTrail is powerful as it shows you relevant events about
            what happened (who stopped the instance -- I have seen this
            take a while to sort out besides being just a tail of the CT
            logs in that region -- e.g. tons of assume roles vs the
            related events

        vi. CloudFormation stack resources shows the relevant stack data

12. This concludes our OpsCenter learning -- One thing you can also due
    under OpsItems \> Configure Sources is change the Severity it is
    alerted as (think of the compliance dashboard we did before),
    Category of the event, and the state

    a.  ![](./media/image33.png){width="6.169231189851269in"
        height="1.153713910761155in"}

13. I hope this shows you the power of OpsCenter as sometimes it is hard
    to see the value in the tooling -- This is complimentary to other
    tools like CWE, Config, and SNS notifications as it allows you to
    easily respond to events with SSM automation runbooks (think Create
    a ServiceNow incident)

Here is an existing tutorial on how to set this up here:

<https://www.youtube.com/watch?v=r6ilQdxLcqY>

This walks you through the initial configuration of OpsCenter and
triggering different events.