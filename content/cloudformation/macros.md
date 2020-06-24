---
weight: 2
title: "Macros"
date: 2019-07-21T21:01:00-05:00
draft: true
---

A CloudFormation [macro](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html) is a type of template transformation that allows you to use a Lambda function to make changes to a template before it is executed.

In this lab, you will create a macro that extends the EC2 resource to automatically create an SSH key for that instance with a name specified in the template. This lab builds on the last one by augmenting the lambda function you wrote to add macro functionality. 

1. Log in to your AWS account and go to the Lambda console

2. Click **Create Function** and choose **Author From Scratch**. 

3. Give it a descriptive name such as 'ssh-key-gen-macro'.

4. Choose Python 3.6 as the **Runtime**

5. Use the role you created earlier with [lambda\_policy.json](../templates/lambda_policy.json)

6. Click **Create Function**

7. Paste in the code from [macro\_lambda.py](../templates/macro_lambda.py) and **Save**. Copy the ARN at the top of the screen.

1. Inspect the code to see what is being done by the function. You will notice that it is mostly the same function from the previous lab, with a new handler added for macro functionality.

    {{< highlight python >}}
        def lambda_handler(event, context):
            "Lambda handler for custom resource and macro"
        
            # Figure out if this is a custom resource request or a macro request
            try:
                if 'RequestType' in event:
                    return custom_resource_handler(event, context)
                else:
                    return macro_handler(event, context)
            except Exception:
                log_exception()
                raise
    {{< /highlight >}}

1. The code searches through the template looking for an EC2 resource with an SSHKey property that starts with 'AutoGenerate-'. If it finds one, it injects the custom resource into the template.

     {{< highlight python >}}
     def macro_handler(event, _):
        "Handler for the template macro"
        
        print(event)
    
        # Get the template fragment, which is the entire starting template
        fragment = event['fragment']
        
        key_name = None
        ec2_resource = None
    
        # Look through resources to find one with type CloudTrailBucket
        for _, r in fragment['Resources'].items():
            if r['Type'] == 'AWS::EC2::Instance':
                for p_name, p in r['Properties'].items():
                    if p_name == 'KeyName':
                        if isinstance(p, str) and p.startswith(AUTO_GENERATE):
                            # We found an EC2 instance with our KeyName property
                            ec2_resource = r
                            key_name = p.replace(AUTO_GENERATE, '')
                            # For the lab we will only support one resource
                            break 
        
        if key_name:
            # Make the EC2 resource depend on the injected custom resource
            if "DependsOn" not in ec2_resource:
                ec2_resource["DependsOn"] = []
            elif isinstance(ec2_resource["DependsOn"], str):
                # We need this to be an array, not a string
                s = ec2_resource["DependsOn"]
                ec2_resource["DependsOn"] = []
                ec2_resource["DependsOn"].append(s)
                
            ec2_resource["DependsOn"].append("SSHKeyCR")
            ec2_resource["Properties"]["KeyName"] = key_name
    
            # Inject the custom resource
            inject_sshkey_resource(fragment, key_name)
    
        # Return the transformed fragment
        return {
            "requestId": event["requestId"],
            "status": "success",
            "fragment": fragment,
        }
    {{< /highlight >}}

2. The resource is injected by simply adding it to the resources dictionary in the fragment

    {{< highlight python >}}
     def inject_sshkey_resource(fragment, key_name):
         'Injects the SSH Key custom resource into the template fragment'
         
         # SSHKeyCR
         custom_resource = {
             'Type': 'Custom::CreateSSHKey',
             'Version': '1.0',
             'Properties': {
                 'ServiceToken': {
                     'Ref': 'FunctionArn'
                 },
                 'KeyName': key_name
             }
         }
     
         fragment['Resources']['SSHKeyCR'] = custom_resource
         
         return True
    {{< /highlight >}}

8. Go to the CloudFormation console. You will create two stacks. The first one is for the macro itself, so that it exists in your selected region as a transform that can be used by any future template.

9. Create the macro stack with [macro\_cfn.yml](../templates/macro_cfn.yml). This will create the macro itself as a unique named resource within your account.

10. Give the stack a unique name such as ssh-key-macro and paste in the ARN from the function you created earlier in this lab into the **FunctionArn** parameter text box. 

11. Fill out the remaining parameters, click **Next**, then **Next** on the following screen. Click **Create** and wait for it to complete. 

1. Inspect the template to see how the macro is created. 

     {{< highlight yaml >}}
       AWSTemplateFormatVersion: "2010-09-09"
       Description: "This template creates the macro"
       Parameters:
         FunctionArn:
           Type: String
       Resources:
         CloudTrailBucketMacro:
           Type: AWS::CloudFormation::Macro
           Properties:
             Name: AutoGenerateSSHKey
             # Paste in the ARN for the function when you create the stack
             FunctionName: !Ref FunctionArn
             
     {{< /highlight >}}

12. Create the EC2 instance and key with [macro\_ec2.yml](../templates/macro_ec2.yml)

13. Give it a unique name like ssh-key-macro-ec2 and fill out the remaining parameters as you did in lab 1.

14. For this template, you will be prompted to create a change set, since the macro is transforming the original template.

1. If you get an error like "Error creating change set: Received malformed response from transform ############::AutoGenerateSSHKey", you might have referenced the wrong lambda function. Check the ARN you pasted in as a parameter to make sure it was the correct function ARN.

15. Once the change set is created, review it to see what resources will be created, and then **Execute**.

1. Inspect the template to see the differences from the previous version. Instead of creating the custom resource explicitly, we invoke the macro to inject it into the template.

     {{< highlight yaml >}}
       AWSTemplateFormatVersion: "2010-09-09"
       Transform: AutoGenerateSSHKey
       ...
       MyEC2Instance:
           Type: AWS::EC2::Instance
           Properties:
             # A key called MyKey01 will be created by a custom resource that is 
             # injected into this template when the transform macro runs
             KeyName: AutoGenerate-MyKey01
             InstanceType: t2.micro
             
    {{< /highlight >}}

16. Download your ssh key from parameter store and confirm that you can log in to your EC2 instance with that key. (Follow the same steps outlined in Lab #1)

17. Delete the stack and the lambda function

18. *Stretch Goal: Modify the CloudFormation template so that you can specify the key name as a parameter.*

