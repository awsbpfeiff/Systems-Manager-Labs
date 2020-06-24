---
weight: 2
title: "Search Software Inventory"
---

#### Resource Inventory in License Manager 
License Manager allows you to discover on-premises applications using Systems Manager inventory, and then to attach licensing rules to them. License Manager periodically collects software inventory, updates licensing information, and refreshes its dashboards to report usage.  After licensing rules are attached to these servers, you can track them along with your AWS servers in the License Manager dashboard.

License Manager cannot, however, validate licensing rules for these servers at launch or termination time. To keep information about non-AWS servers up-to-date, you must periodically refresh the inventory information using the Search inventory section of the License Manager console.

Systems Manager stores data in its Inventory data for 30 days. During this period, License Manager counts a managed instance as active even if it is not pingable. After inventory data has been purged from Systems Manager, License Manager marks the instance as inactive and updates local inventory data. To keep managed instance counts accurate, we recommend manually deregistering instances in Systems Manager so that License Manager can run cleanup operations.

Resource inventory tracking is also useful if your organization does not restrict AWS users from creating AMI-derived instances or installing additional software on running instances. License Manager provides you with a mechanism to easily discover these instances and applications using inventory search. You can attach rules to these discovered resources and track and validate them the same as instances created from managed AMIs. 

**Using Inventory Search**

Complete the following steps to search your resource inventory. You can search for applications by name (for example, names that begin with "SQL Server") and the type of license included (for example, a license that is not for "SQL Server Web").

**To search your resource inventory**

1. Open the License Manager console at https://console.aws.amazon.com/license-manager/.
2. In the navigation pane, choose **Search inventory**.
3. Use the **Application name** filter to scope the list of resources.  We will search using the following two filters.
4. First, we will search for **Application name** Equals **Microsoft SQL Server 2016**.
![](../images/discoverysqlserver.png)
5. Before we search for the second application,  we will clear the filter.
6. Then, we will search for **Application name** Equals **oracle-database-xe-18c**
![](../images/searchoracle.png)

Searching software inventory allows you to find software installed on Systems Manager Managed Instances and associate them with a license configuration for tracking.  This is especially useful for on premises instances that
you can't track by using AMIs.

 
