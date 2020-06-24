---
weight: 3
title: "Intermediate Labs"
---

---


### Intermediate Labs - Service Action
  ### Give the user the ability to restart the server from Service Catalog (Service Actions)

1.  Switch Role to the **SwitchRoleSCAdmin** role

2.  Copy the url for **SwitchRoleSCAdmin** and use it in a new browser
    tab

    > you should still have the url in a text box on this page if you scroll
    > up ![](../images/image11.png)

3.  Return to [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home)

    > ![](../images/image12.png)

4.  Choose **Service actions**

    > ![](../images/image13.png)

5.  Choose **Create new action** button

6.  Choose **AWS-RestartEC2Instance**

7.  Choose **Next**

8.  On the **Configure** page use defaults , choose **Create action**

9.  Choose the **AWS-RestartEC2Instance** choose **Associate action**

    > ![](../images/image14.png)

10. Choose the product **LABEC2** choose **V1** version, choose
    **Associate action**

    > Great !! users can now restart their servers by themselves

### End-user Restart a server 

1.  Switch back to the End-user Role

    > ![](../images/image15.png)

2.  Choose the role name, choose **ServiceCatalogEndUser**

3.  Choose **Switch Role**

4.  Return to [Service
    Catalog](https://console.aws.amazon.com/servicecatalog/home)

5.  Choose **Provisioned product list**

6.  Choose a **Provisioned Product name**

    > ![](../images/image16.png)

7.  Choose the **ACTIONS** button, Choose **AWS-RestartEC2Instance**

8.  Choose **RUN ACTION**

    > ![](../images/image17.png)

9.  Switch to the [EC2
    Console](https://console.aws.amazon.com/ec2/v2/home) Look for the
    server restarting

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 