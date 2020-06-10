This section will cover creating a Resource Group and some tags that
will be used later on in the Patch Manager lab.

1.  Navigate to the Systems Manager Console \> Application Management \>
    [Resource
    Groups](https://console.aws.amazon.com/systems-manager/resource-groups)

2.  Create a resource group (on the right)

3.  From navigation panel on the left, under **Tagging** select **Tag
    Editor**

    a.  Region = us-east-1

    b.  Resource types = AWS::EC2::Instance

    c.  Select **Search resources**

    d.  Select 2 of the 4 instances \> Select **Manage Tags of selected
        resources**

    e.  Add tag

        i.  Key = Patch Group

        ii. Value = App

    f.  Select **Review and apply tag changes**

4.  From navigation panel on the left, under **Resources** select
    **Create Resource Group**

    a.  Select **Tag based**

    b.  Under Resource Types select **AWS::EC2::Instance**

    c.  Under Tags enter

        i.  Key = Patch Group

        ii. Value = App

    d.  Select **View Group Resources**

![](./media/image3.png)

5.  In the group name enter **App**

6.  In group tags you can add more tags to the full group if you would
    like -- Key = App / Value = Front-end

    a.  These group tags do not show up under the individual resources
        -- It is another layer of tagging that can be done to the
        Resource Group

7.  Select **Create Group**

8.  Repeat the above steps for the remaining 2 Instances but tag them as
    **Web**
