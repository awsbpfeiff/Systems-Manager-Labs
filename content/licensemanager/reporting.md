---
weight: 5
title: "Reporting"
---

The Dashboard section of the AWS License Manager console provides graphs to track the license consumption associate with each license configuration. The dashboard also displays alerts resulting from license rule violations.

The following information is available in the graph for a license configuration:

* License configuration name
* License type
* Licenses consumed
* Number of licenses remaining
* Whether the rules are enforced
* Number of hosts for each tenancy type

Click on [License Manager Dashboard](https://console.aws.amazon.com/license-manager/home?#/dashboard) to access the dashboard.

APIs are also available to create custom reports.  Below are the API actionns that can be used in customizations.

The following actions are supported:

* CreateLicenseConfiguration
* DeleteLicenseConfiguration
* GetLicenseConfiguration
* GetServiceSettings
* ListAssociationsForLicenseConfiguration
* ListLicenseConfigurations
* ListLicenseSpecificationsForResource
* ListResourceInventory
* ListTagsForResource
* ListUsageForLicenseConfiguration
* TagResource
* UntagResource
* UpdateLicenseConfiguration
* UpdateLicenseSpecificationsForResource
* UpdateServiceSettings

**Note:** Additional details on APIs can be found [here](https://docs.aws.amazon.com/license-manager/latest/APIReference/API_Operations.html).

