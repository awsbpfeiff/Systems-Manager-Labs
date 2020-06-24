---
title: "State Manager"
date: 2020-06-T14:10:29-04:00
weight: 8
chapter: false
pre: "<b>8. </b>"
---

**State Manager** is a tool that allows administrators to Maintain a
consistent configuration of your instances and applications. **State
Manager** allows you to specify a schedule to reapply your
configurations to your instances. **State Manager** provides similar
functionality to traditional Configuration Management tooling like
Puppet, Chef, and Ansible. You specify a schedule to apply your
**Document** commands to your **Managed Instances** via an
**Association**. In this lab, we will be enabling the pre-defined
**Document** for updating the SSM Agent on targeted **Managed
Instances.**

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

8. If this was a real world scenario you would like check weekly for a
    new release

9. **Compliance:** High

10. This is specifying how you like this ranked within the Compliance
    dashboard -- if the agent is not updated you will see a High
    severity non-compliance alert

11. **Rate Contro:** Target 1 and Error 1

12. Leave writing output to S3 bucket unchecked for now

13. Select Create Association

14. ![](./media/image10.png)

15. Select on the Association ID to review the Association details

16. Then Select Apply Association Now (upper right corner)

17. Select **Apply**

18. Select the Association ID to review the Association details

19. Select **Execution History**

20. Select the most recent **Execution ID**

![](./media/image11.png)

21. Select the **Output** of one of the Resource IDs

![](./media/image12.png)

22. A new tab will open -- Expand **Step 1 -- Output**

![](./media/image13.png)

23. You can see that output of the **Document** being executed and
    updating the SSM Agent

24. If there are no updates the installation is skipped
