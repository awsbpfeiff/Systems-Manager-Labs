---
weight: 3
title: "Mapping and StackSets"
date: 2019-07-21T21:01:00-05:00
---

#### Mappings

The mappings section of a CloudFormation template matches a key to a corresponding set of named values. For example, if you want to set values based on a region, you can create a mapping that uses the AWS region name as a key and contains the values you want to specify depending on the region where the template is deployed. This may be particularly useful when deploying AMIs globally where you must deploy a different AMI ID per region due to disaster recovery or security considerations that differ across geographic regions. 

#### StackSets

[AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-concepts.html) extend the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation. Using an administrator account, you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts across specified regions. For example, you can easily establish a global AWS CloudTrail or AWS Config policy across multiple accounts with a single StackSet operation. You can also use StackSets to deploy resources in a single account across multiple regions. 

In this lab, we will deploy a simple CloudFormation template that provisions an EC2 instance with a simple webserver to verify proper deployment across multiple regions. We will use mappings to properly deploy the proper Amazon Linux 2 AMI for the selected region while using StackSets to configure which AWS regions will deploy this template.

For the sake of simplicity, we will be utilizing one account as both the administrator and execution role but you can utilize StackSets across multiple accounts. Please review the [Prerequisites: Granting Permissions for Stack Set Operations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs.html) page for additional information on how to properly configure the two roles required to deploy StackSets across multiple accounts. 

#### Deploy StackSet IAM Permissions

1. Log into your AWS account from the N. Virginia region and click the role selection menu. Click "My Account" and take note of your AccountId. We will need this to properly establish IAM permissions. 

2. Navigate to the CloudFormation console. 

3. Click **Create Stack**

4. Select **Upload a template to Amazon S3** and upload the [mapping\_stacksets\_iam.yml](../templates/mapping_stacksets_iam.yml) CloudFormation template. 

1. Examine the CloudFormation template to see that it is a simple nested CloudFormation template that will call two AWS-provided YAML files to provision IAM roles within your account. 

    {{< highlight yaml >}}
    AWSTemplateFormatVersion: '2010-09-09'
    Description: This CloudFormation StackSet deploys two AWS provided CloudFormation templates that add Administrator and Execution Roles required to use AWSCloudFormationStackSetAdministrationRole
    Resources:
    AWSCloudFormationStackSetAdministrationRole:
    Type: AWS::CloudFormation::Stack
    Properties:
    TemplateURL: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetAdministrationRole.yml 
    TimeoutInMinutes: '3'
    AWSCloudFormationStackSetExecutionRole:
    Type: AWS::CloudFormation::Stack
    Properties:
    TemplateURL: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml 
    TimeoutInMinutes: '3'
    Parameters:
    AdministratorAccountId : !Ref 'AccountID'
    Parameters:
    AccountID:
    Type: String
    Description: Your AWS Account ID
    MaxLength: 12
    MinLength: 12
    {{</ highlight >}}
    
    You can review the resources deployed in each template here:

    [AWSCloudFormationStackSetAdministrationRole](https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetAdministrationRole.yml)
    
    [AWSCloudFormationStackSetExecutionRole](https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml)

5. Click **Next**.

6. Give your CloudFormation stack a descriptive name such as 'mapping-stacksets-iam'.

7. Specify the AccountID of your AWS account that you obtained earlier and click **Next**.

8. Review any desired tags and additional options and click **Next** 

9. Click the checkbox next to 'I acknowledge that AWS CloudFormation might create IAM resources with custom names.' and then click **Create**.

10. Click the refresh button to see that the two nested CloudFormation templates were successfully deployed into your account.

11. Do not delete these nested stacks until the end of the next session as they are the permissions necessary to deploy a StackSet in your account. 

#### Deploy StackSet with Mapping

1. Navigate to the CloudFormation console and from the CloudFormation drop-down, select **StackSets**.

2. Click **Create StackSet**.

3. Examine the AWS-provided CloudFormation sample templates to get an idea of what configuration operations are possible with StackSets and then click 

#### Upload a template to Amazon S3

4. Click **Browse**, then upload [mapping\_stacksets\_ec2.yml](../templates/mapping_stacksets_ec2.yml) and then click **Next**.

1. Notice the mapping component of this template that will deploy the proper Amazon Linux 2 AMI based on the region where the template is deployed.

    {{< highlight yaml >}}
    Mappings:
    # Mapping of Amazon Linux 2 AMI IDs in every AWS Region
    # When deploying a StackSet, the template will automatically deploy the proper AMI in each selected region
    RegionMap:
        us-east-1: 
        AMI: ami-04681a1dbd79675a5
        us-east-2: 
        AMI: ami-0cf31d971a3ca20d6
        us-west-1:
        AMI: ami-0782017a917e973e7
        us-west-2:
        AMI: ami-6cd6f714
        ap-south-1:
        AMI: ami-00b6a8a2bd28daf19
        ap-northeast-3:
        AMI: ami-00f7ef6bf92e8f916
        ap-northeast-2:
        AMI: ami-012566705322e9a8e
        ap-southeast-1:
        AMI: ami-01da99628f381e50a
        ap-southeast-2:
        AMI: ami-00e17d1165b9dd3ec
        ap-northeast-1:
        AMI: ami-08847abae18baa040
        ca-central-1:
        AMI: ami-ce1b96aa
        eu-central-1:
        AMI: ami-0f5dbc86dd9cbf7a8
        eu-west-1:
        AMI: ami-0bdb1d6c15a40392c
        eu-west-2:
        AMI: ami-e1768386
        eu-west-3:
        AMI: ami-06340c8c12baa6a09
        sa-east-1:
        AMI: ami-0ad7b0031d41ed4b9
    {{< /highlight >}}

    > *Note: The EC2 instance that is deployed in this template will use the default VPC*

5. Give your CloudFormation stack a descriptive name such as 'mapping-stacksets-ec2', provide a source CIDR IP range that you want to allow HTTP traffic from to verify operation of your web server and then click **Next**. 

    > *We recommend that you provide the smallest IP range possible depending on your network configuration.*

6. Under "Specify Accounts", click "Deploy stacks in accounts" and provide your AWS AccoundId that you obtained earlier. This input accepts multiple account numbers as well as *.csv files so that you can scale your configuration to multiple accounts. 

7. Scroll down and highlight the regions where you would like to deploy the resources described in the [mapping\_stacksets\_iam.yml](../templates/mapping_stacksets_iam.yml) CloudFormation template. The order that regions are selected in the StackSet console will be maintained such that this will be the same order that regions are provisioned when deploying the StackSet. Take note of the regions where you choose to deploy these resources. We recommend you deploy to at least two regions. The more regions you select, the longer the StackSet will take to deploy. 

    > *NOTE: This template will deploy resources into the default VPC in each selected region. Please select regions where there is still a default VPC*

8. Scroll down and click **Next** 

9. Note that the IAM Administration and IAM Execution Roles are pre-populated. Click **Next**.

10. Review the details of this StackSet deployment and click **Create**.

11. Under the Operations section, you will see the status of this StackSet as running. Under Stacks, you will see an entry for each region you selected when deploying the StackSet. Allow approximately 5 minutes for this deployment to complete. The operation will be complete when all selected regions display a status as "CURRENT".

12. Navigate to the CloudFormation console in one of the regions where you deployed the StackSet. Select Stacks from the CloudFormation dropdown menu and select the StackSet. 

13. On the Outputs tab, click the link to the "Webserver URL" to verify that your webserver successfully deployed in this region.

14. Change to each of the remaining regions that you chose to deploy the StackSet to and repeat step 13.

#### Delete StackSet

1. Navigate back to the region where you deployed the StackSet (N. Virginia) and return to the StackSet console. StackSets are deployed globally but reside only in the region where they were initially deployed.

2. Click the StackSet name.

3. Click **Manage StackSet**.

4. Click **Delete Stacks** and then click **Next**.

5. Enter your AWS account ID into the "Delete stacks from account" input.

6. Click **Add all** to select all regions for deletion and then scroll down to click **Next**.

7. Click **Delete Stacks**.

8. Refresh this page and note that the DELETE operation is running. Allow approximately 5 minutes for this operation to complete across all regions and then navigate to the StackSets console. The operation is finished when there are no regions present under the Stacks section. 

9. Select the StackSet. Under actions, select **Delete StackSet** and then click **Yes, Delete**. If you receive a "Failed to delete StackSet" error, please allow more time for the underlying stacks to be deleted. 

10. Return to the CloudFormation Stacks console and delete the parent mapping-stacksets-iam (or similar) template that you deployed in the first step of this lab. Be sure to select the parent Stack, not the "NESTED" stacks.