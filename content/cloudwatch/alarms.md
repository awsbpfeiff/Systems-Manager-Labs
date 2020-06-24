---
weight: 5
title: "Create alarms"
---

#### Create Alarms for Known Issues from Metrics
 
Monitoring for Application Outcomes Ryn Brandish wants to understand when there are application issue and is concerned about security. He would like metrics for errors and warnings. ExampleCorp has limitations on the image size that it can successfully process. While this issue does not happen frequently it does not make customers happy. Being aware of the error rate for submitted images will allow the Business and Development team to determine if increase the image size should be a priority. Currently most warnings are related to security issues. Ryn would like visibility on how often they happen. 
 
**5.1 Create a log filter metric based on the defined threshold for the error** 
 
1. Click on Log Groups in the breadcrumb trail at the top of the screen 
2. Select the application.log log group 
3. Click on the Create Metric Filter button 
4. Enter "ActiveStorage::InvariableError" (include quotes) for Filter Pattern. This is a known error that we will cause later in the lab. 
5. Click on the Assign Metric button 
6. Enter ImageError as the Metric Name 
7. Click on the Create Filter button 
![Image Error Metric](../images/imageerrormetric.jpg) 
 
**5.2 Create an alarm for the error when the metric threshold is crossed** 
 
1. Click on Create Alarm 
2. Click Edit by Metric 
![Edit Metric](../images/editmetric.jpg) 
3. Change Period to 10 seconds 
4. Click on Select metric button 
    - Enter ImageErrorAlarm for Name 
    - After is >=, change the 0 to 1 
    - Select good (not breaching threshold) for Treat missing data as 
    - Under Actions Select from the Drop Down the ExampleCorpErrorNotify* list to Send notifications to 
    - Enter [your email address] for Email list 
![Create Alarm](../images/AlarmDetails.jpg) 
5. Click on the Create Alarm button 
6. Click on the I will do it later button when asked to verify the email address 
     
**NOTE:** You should see that the alarm has insufficient data with a 1 next to INSUFFICIENT in the navigation menu. This will change to OK within a few seconds. 
 
**5.3 Generate error logs through user activity** 
 
1. Navigate to the ExampleCorp using the URL that you made a note of earlier (CloudFormation Output). 
2. Enter admin@admin.com for email 
3. Enter Password123 for Password 
4. Click Login 
5. Click Upload Image Click Select Image 
7. Find break_app.jpg in your filesystem 
8. Click Upload 
     
**NOTE:** The application will start exhibiting issues, and the application will eventually fail to load. 
 
**5.4 Review logs to identify error** 
 
1. Navigate to the CloudWatch Console 
2. Click Logs  in the navigation menu 
3. Click on application.log log group - this is from /opt/ExampleCorp/log/application.log as configured earlier in the CloudWatch agent configuration. 
4. Click on the Search Log Group button 
5. Enter ERROR (case sensitive) and hit enter. 
6. You will see errors ending ActiveStorage::InvariableError. This is an error that is generated when a user uploads a non-image file. This is a known issue, but the Dev team has not had cycles to adress it. 
 
### **Build a Monitoring Plan: Alerting and Response** 
 
Monitoring for Operational Outcomes Sansa Bailish is focused on outages, reliability, and getting better sleep. There is a known issue with image trends; when the instances are rebooted the application doesn't restart. Too frequently Sansa has received a late night call to get online and restart the application. The user experience lead is very frustrated with the downtime associated to these incidents. They need to be detected sooner and resolved faster. 
 
Sansa is looking for a monitoring solution to detect the incident (the reboot event), and a way to trigger an automated recovery. 
 