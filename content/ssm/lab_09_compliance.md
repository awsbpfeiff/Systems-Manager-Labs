---
title: "Compliance"
date: 2020-06-T14:10:29-04:00
weight: 11
chapter: false
pre: "<b>11. </b>"
---

Compliance is intended to be a dashboard where you can influence what is
important to you and your organization by specifying a Severity level
during configuration and present on if it is compliant with the task.
For example, you might have an Association that you want to know is
being applied to your instance (host firewall configuration). Another
example would be if your instances have been patched according to the
configured Patch Baseline.

1.  Navigate to [Systems Manager \> Instances & Nodes \>
    Compliance](https://console.aws.amazon.com/systems-manager/compliance)

2. Compliance will allow you to sort by Compliance Type, Patch Group,
    or Resource Group

3. Sort by Resource Group

4. You can now review the compliance status of the **Resource Group**
    we created earlier in the lab

![](./media/image15.png)

5. If you have thousands of instances this view can get overwhelming --
    You can further drill down the results by using the **Filter
    Further** search bar

![](./media/image16.png)
6. Any time you see an item for **Compliance** within a dashboard, this
    is where you can influence how that information is displayed within
    this dashboard (Critical, high, medium, and low)
