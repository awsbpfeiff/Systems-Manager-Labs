Explorer builds on top of OpsCenter and enables you to have a
multi-region / multi-account view of your environment.

1.  Navigate to [Systems Manager \> Operations Management \>
    Explorer](https://console.aws.amazon.com/systems-manager/explorer?region=us-east-1)

2.  Select **Get started**

3.  For tags for reporting -- remove the cloudformation default key (we
    can configure these later on) and select **Enable Explorer**

    a.  ![](./media/image37.tiff){width="5.571071741032371in"
        height="2.3692311898512686in"}

4.  You are presented with an option to configure resource data sync --
    Since we are in the lab and using a single account / single region
    this won't be necessary but worth visualizing

    a.  ![](./media/image38.png){width="6.719466316710411in"
        height="3.823076334208224in"}

    b.  This makes it easy to select:

        i.  Select regions you are active in

        ii. Select all accounts in an org

        iii. EVEN COOLER select OUs within an org

5.  Select **Settings** (top right) \> Under Tags for Reporting \> We
    can enter Patch Group and SSM Managed and then select **Save**