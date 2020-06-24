---
weight: 1
title: "Custom Resources"
---

You can extend the capabilities of CloudFormation with [custom resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) by delegating work to a Lambda function that is specially crafted to interact with the CloudFormation service. In your code, you implement the create, update, and delete actions, and then you send a response with the status of the operation.

In this lab, you will create a custom resource that generates an SSH key and stores it in SSM parameter store.

**Note - This lab requires you to be in the N.Virginia region**

1. Log in to your AWS account, select the N. Virginia region, and go to the Lambda console

2. Click **Create Function** and choose **Author From Scratch**. 

3. Give it a descriptive **Name** such as 'ssh-key-gen'.

4. Choose Python 3.6 as the **Runtime**

5. Choose **Create a Custom Role**. A new tab will open. 

6. Give the role a name such as 'ssh-key-gen-role', expand **View Policy Document**, click **Edit**, and paste in the policy from [templates\_policy.json](../templates/lambda_policy.json). 

7. **Click Allow** to associate the new role with the lambda function.

   1. Examine the policy to see what permissions we are granting to the new function

        {{< highlight json >}}
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "logs:CreateLogGroup",
                        "logs:CreateLogStream",
                        "logs:PutLogEvents"
                    ],
                    "Resource": "arn:aws:logs:*:*:*"
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "ec2:CreateKeyPair",
                        "ec2:DescribeKeyPairs",
                        "ssm:PutParameter"
                    ],
                    "Resource": "*"
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "ec2:DeleteKeyPair",
                        "ssm:DeleteParameter"
                    ],
                    "Resource": "*"
                }
            ]
        }
        {{< /highlight >}}

8. Click **Create Function** and then paste in the code from [custom_resource_lambda.py](../templates/custom_resource_lambda.py) into the editor.

   1. Examine the function to see what we are doing to implement the custom resource.

      Every lambda function has a handler that is called by the lambda environment in response to a trigger.

      {{< highlight python >}}
      def lambda_handler(event, context):
          "Lambda handler for the custom resource"
      
          try:
              return custom_resource_handler(event, context)
          except Exception:
              log_exception()
              raise
       {{< /highlight >}}

      The handler function must determine the type of request and send a response back to CloudFormation. In this code snippet, we handle a create request from CloudFormation.

       {{< highlight python >}}
           if event['RequestType'] == 'Create':
              try:
                  print("Creating key name %s" % str(pem_key_name))
      
                  key = ec2.create_key_pair(KeyName=pem_key_name)
                  key_material = key['KeyMaterial']
                  ssm_client = boto3.client('ssm')
                  param = ssm_client.put_parameter(
                      Name=pem_key_name, 
                      Value=key_material, 
                      Type='SecureString')
      
                  print(param)
                  print(f'The parameter {pem_key_name} has been created.')
      
                  response = 'SUCCESS'
      
              except Exception as e:
                  print(f'There was an error {e} creating and committing ' +\
                      f'key {pem_key_name} to the parameter store')
                  log_exception()
                  response = 'FAILED'
      
              send_response(event, context, response)
      
              return
      {{< /highlight >}}

   2. Responses are sent to the CloudFormation endpoints as HTTPS PUTs. This code snippet uses urllib, which is a part of the Python 3 standard library.

    {{< highlight python >}}
      def send_response(event, context, response):
          "Send a response to CloudFormation to handle the custom resource lifecycle"
      
          responseBody = { 
              'Status': response,
              'Reason': 'See details in CloudWatch Log Stream: ' + \
                  context.log_stream_name,
              'PhysicalResourceId': context.log_stream_name,
              'StackId': event['StackId'],
              'RequestId': event['RequestId'],
              'LogicalResourceId': event['LogicalResourceId'],
          }
      
          print('RESPONSE BODY: \n' + dumps(responseBody))
      
          data = dumps(responseBody).encode('utf-8')
          
          req = urllib.request.Request(
              event['ResponseURL'], 
              data,
              headers={'Content-Length': len(data), 'Content-Type': ''})
          req.get_method = lambda: 'PUT'
      
          try:
              with urllib.request.urlopen(req) as response:
                  print(f'response.status: {response.status}, ' + 
                        f'response.reason: {response.reason}')
                  print('response from cfn: ' + response.read().decode('utf-8'))
          except urllib.error.URLError:
              log_exception()
              raise Exception('Received non-200 response while sending ' +\
                  'response to AWS CloudFormation')
      
          return True
    {{< / highlight >}}

9. Click **Save** to save the function. Copy the ARN at the top right of the screen.

![](../images/arn.png)

11. Go to the CloudFormation console and click **Create Stack**

12. Select **Upload a template to Amazon S3** and upload [custom_resource_cfn.yml](../templates/custom_resource_cfn.yml) 

    1. Examine the CloudFormation template to see how a custom resource is configured.

     {{< highlight yaml >}}
       Resources:
         SSHKeyCR:
             Type: Custom::CreateSSHKey
             Version: "1.0"
             Properties:
               ServiceToken: !Ref FunctionArn
               KeyName: !Ref SSHKeyName
     {{< /highlight >}}

13. Give the stack a unique name such as ssh-key-gen-cr and paste in the ARN from the function you created earlier into the **FunctionArn** parameter text box. 

14. Fill out the remaining parameters, Click **Next**, then **Next** on the following screen.

15. Check the box that reads **I acknowledge that AWS CloudFormation might create IAM resources.** and then click **Create**.

16. Once the stack has completed creation (it might take a few minutes), go to the EC2 console and confirm the creation of your new instance.

17. Go to the Systems Manager Console, view Parameter Store and confirm that the key has been stored. It was stored with the Secure String setting, which uses KMS to encrypt the parameter value.

18. Download your SSH key from Parameter Store (***not*** the EC2 console!) and store it in a .pem file with permissions set to 600 on Linux or Mac. You can download it from the console by selecting the key and clicking **Show** under the **Value** at the bottom of the screen. Copy-paste everything beginning with -----BEGIN RSA PRIVATE KEY----- and ending with -----END RSA PRIVATE KEY-----

19. As an alternative to using the console to copy and paste the key, use the following shell commands (assuming that you kept the default key name of MyKey01, and that you have a .ssh directory in your home directory)

    {{< highlight bash >}}
    aws ssm get-parameter --name MyKey01 --with-decryption \
        --query Parameter.Value --output text > ~/.ssh/MyKey01.pem
    
    chmod 600 ~/.ssh/MyKey01.pem
    
    ssh -i ~/.ssh/MyKey01.pem ec2-user@<PUBLIC DNS>
    {{< / highlight >}}

20. **NOTE that this is a private key, and in a production setting you must take steps to ensure that this key is not compromised!**

21. Log in to the newly created EC2 instance to confirm that the key was associated with the instance.

22. Delete the stack and the lambda function.
