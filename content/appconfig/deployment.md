---
weight: 8
title: "Deployment"
---

#### Deploy Configuration

Now that we have created our AppConfig Application, our Environment for that application and a Configuration profile that references our Configuration Document in S3, we need to deploy it to make it available to any applications that needs to use our configuration data.

Before we continue with deployment, we first need to create a Deployment Strategy to govern how quickly the deployment will progress to becoming available to applications that will consume our configuration data.

We will configure our Deployment Strategy to deploy our configuration data linearly to 20% of our hosts at a time with a total deployment time of 1 minute. This will result in a deployment of 20% to our hosts every 12 seconds. Additionally, we will configure a Bake time of 1 minute.  

**Note: We are intentionally choosing a very short deployment feedback loop for the purposes of this lab so that we can have time to cover more topics.**

Use the following procedure to create a deployment strategy and deploy our configuration data.

1. From the configuration profile details page, select **Start deployment**.

2. Choose **AppConfigLabLambdaDevelopment** from the Environment dropdown list.

3. Select the Latest version from the **S3 object version** dropdown list.

4. Select **Create deployment strategy** and enter **AppConfigLabLinearTwentyPercentDeploymentStrategy** in the **Name** field and provide an optional **Description**.

5. Select **Linear** for the **Deployment type** field.

6. Enter **20** for the **Step percentage** field.

7. Enter **1 Minute(s)** for the **Deployment time**.

8. Enter **1 Minute(s**) for the **Bake time**. ![](../images/appconfig-deployment-strategy.png) 

9. Select **Create deployment strategy** to save the deployment strategy and return to the **Start deployment** page. 

10. Reselect **AppConfigLabLambdaDevelopment** from the **Environment** dropdown list if cleared after creating the deployment strategy.

11. Enter an optional **Deployment description**. ![](../images/appconfig-deployment.png?width=70pc)

12. Select **Start deployment** and observe the Percentage complete as it proceeds to 100% and note the transition of the **State** field to **Baking** and eventually to **Completed**. ![](../images/appconfig-deployment-status.png?width=70pc)

At this point in the lab, we have created an AppConfig application, a Configuration Document and uploaded it to S3, created a development environment and created a deployment strategy to govern the roll out and completed deployment.

In the next section, we will build an API Gateway application in which we will consume the configuration data. 
