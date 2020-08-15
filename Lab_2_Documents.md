An AWS Systems Manager document (SSM document) the configuration options, policies, and the actions that Systems Manager performs on your managed instances and other AWS resources. Systems Manager includes more than a hundred pre-configured documents that you can use by specifying parameters at runtime. Documents use JavaScript Object Notation (JSON) or YAML, and they include steps and parameters that you specify.

There are multiple document types for different Systems Manager capabilities.  They can be reviewed here: 

[Systems Manager Documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html)

You can use pre-defined AWS managed documents or create your own depending on your use case.

In this section we will create a custom document that can be used with other Systems Manager capabilities Run Command and State Manager.

1. Open the AWS Systems Manager console at https://us-east-1.console.aws.amazon.com/systems-manager.
1. In the navigation pane, select **Documents** under the **Shared Resources** section.
1. Inside here you will be able to see all documents available to your account for the given AWS Region. There are four different tabs:
    - **Owned by Amazon:** Managed Documents published and maintained by AWS.
    - **Owned by me:** Custom Documents your organization has created.
    - **Shared with me:** Documents that you have been granted access to for the given AWS Region.
    - **All documents:** Display all documents available to your account for the given AWS Region.
1.  Select **Create Command or session**
    - For **Name**, enter ```org-install-app```.
    - For **Target type - *optional***, leave the value blank for now.
        - Target Type allows you to restrict the types of resources the document can run against.
    - For **Document type - *optional***, leave **Command document** as we will use Run command to install the package.
    - For **Content**, copy and paste the below snippet:
```
{
    "schemaVersion": "2.2",
    "description": "Command Document Example JSON Template",
    "parameters": {
        "Message": {
            "type": "String",
            "description": "Preparing Web Instance",
            "default": ""
        }
    },
    "mainSteps": [{
        "action": "aws:runShellScript",
        "name": "prepare-web-instance",
        "inputs": {
            "runCommand": [
                "echo {{Message}}",
                "sudo yum install httpd -y",
                "mkdir /app",
                "touch /app/hello.txt",
                "sudo systemctl start httpd"
            ]
        }
    }]
}
```

1. Choose **Create Document** to save the document.
1. Choose the **Owned by me** tab and selec the new document you created, ```org-install-app```.
    - Choose the **Content** tab and review the contents of the document. We will run this document on our managed instances using **Run command**.