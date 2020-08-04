An AWS Systems Manager document (SSM document) defines the actions that Systems Manager performs on your managed instances. Systems Manager includes more than a dozen pre-configured documents that you can use by specifying parameters at runtime. Documents use JavaScript Object Notation (JSON) or YAML, and they include steps and parameters that you specify.

There are multiple document types for different Systems Manager capabilities.  They can be reviewed here: 

[Systems Manager Documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html)

You can use pre-defined AWS managed documents or create your own depending on your use case.

In this section we will create a custom document that can be used with other Systems Manager capabilities like Distributor, Run, and State Manager.

1.  Navigate to the [Systems Manager
    Console](https://us-east-1.console.aws.amazon.com/systems-manager/documents) \>
    Shared Resources \> Documents

2.  Inside here you will be able to see all documents available to your
    account in the region you are presently logged into

    a.  **Owned by Amazon:** Managed Documents published and maintained by
        AWS

    b.  **Owned by me:** Custom Documents your organization has created

    c.  **Shared with me:** Documents that you have been granted access to
        within that region

    d.  **All documents:** All of the above

3.  Navigate to Owned by me

4.  Select **Create Command or Session**

    a.  **Name:** org-install-app

    b.  **Target Type:** Leave blank for now -- This allows you to
        narrow down the resources the doc can run against

    c.  **Document Type:** Command document as we will use Run command
        to install the package

    d.  Copy the below snippet into the contents:
    
```
    {
    	"schemaVersion": "2.2",
    	"description": "Command Document Example JSON Template",
    	"parameters": {
    		"Message": {
    			"type": "String",
    			"description": "Magic",
    			"default": "Prepping Application Instance"
    		}
    	},
    	"mainSteps": [{
    		"action": "aws:runShellScript",
    		"name": "Magic",
    		"inputs": {
    			"runCommand": [
    				"sudo yum install httpd -y",
    				"mkdir /app",
    				"touch /app/hello.txt",
    				"sudo systemctl start httpd"
    			]
    		}
    	}]
    }
```

5.  Select **Create Document**

6.  Browse to Documents \> Owned by me and confirm that the new document
    exists

    a.  Select on the name of the document

    b.  Browse to Content and review the contents of the document -- We
        will use this with the Run command
