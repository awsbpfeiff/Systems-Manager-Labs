AWS Systems Manager State Manager is a secure and scalable configuration management service that automates the process of keeping your Amazon EC2 and hybrid infrastructure in a state that you define.

The following list describes the types of tasks you can perform with State Manager.

* Bootstrap instances with specific software at start-up

* Download and update agents on a defined schedule, including SSM Agent

* Configure network settings

* Join instances to a Windows domain (Windows Server instances only).

* Patch instances with software updates throughout their lifecycle

* Run scripts on Linux and Windows managed instances throughout their lifecycle

In this section we will configure a **State Manager Association** that will update the Systems Manager Agent on the registered Managed Instances.  

1.  Navigate to [Systems Manager \> Instances & Nodes \> State
    Manager](https://console.aws.amazon.com/systems-manager/state-manager)

2. Select Create Association (top right)

3. **Name:** UpdateSSMAgent

4. Search for **AWS-UpdateSSMAgent** and select that as the Document
    for the Association

5. Parameters leave as the Default (false)

6. **Targets:** Selecting all managed instances in this region under this
    account

7. **Specify schedule:** On schedule (can run one for initial
    provisioning) / every 30 mins (for the lab)

   1. If this was a real world scenario you would configure the frequency to be 14 days per Systems Manager best practices

8. **Compliance:** High

9.  This is specifying how you like this ranked within the Compliance
    dashboard -- if the agent is not updated you will see a High
    severity non-compliance alert

10. **Rate Control:** Target 1 and Error 1

11. Leave writing output to S3 bucket unchecked for now

12. Select Create Association

13. ![](./media/image10.png)

14. Select on the Association ID to review the Association details

15. Then Select Apply Association Now (upper right corner)

16. Select **Apply**

17. Select the Association ID to review the Association details

18. Select **Execution History**

19. Select the most recent **Execution ID**

![](./media/image11.png)

21. Select the **Output** of one of the Resource IDs

![](./media/image12.png)

22. A new tab will open -- Expand **Step 1 -- Output**

![](./media/image13.png)

23. You can see that output of the **Document** being executed and
    updating the SSM Agent

24. If there are no updates the installation is skipped
