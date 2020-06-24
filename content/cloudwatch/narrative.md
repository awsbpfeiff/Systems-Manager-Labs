---
weight: 2
title: "Narrative"
---

#### Review the ExampleCorp Requirements and Build a Monitoring Plan 
 
**2.1 Review the ExampleCorp \[fictional\] Narrative** 
 
In the lab we will use a mock workload for a fictitious company to learn about performing operations on AWS. Details on the application used in this lab can be found on GitHub at this link. 
 
__*About Us:*__ ExampleCorp was started on a simple idea: “A picture is worth a thousand words”. We help users find out how many words that picture is worth. Our users make thousands of connections every day based on what they discover in the pictures they share. We want to make sharing pictures and identifying what lies within, faster and easier for them. 
 
__*Our App:*__ ExampleCorp has one primary application – a photo library that tags the contents of images users upload, through machine learning. Through our application users can upload pictures of themselves and friends to see which celebrities they resemble, and find celebrities in photos they upload. We have >5 million users (500,000 active daily). 
 
__*Our Architecture:*__ The ExampleCorp application is a simple Ruby on Rails app with a MySQL database. It is installed as two Docker containers (application and database) launched in a VPC on EC2 instances using CloudFormation. Users upload images and background jobs analyze them and collect metadata. These jobs use AWS Rekognition to detect labels, text and celebrities, and EXIF metadata from the camera used. Deployment of application code uses update-in-place. 
 
*Our Operating Model:* The ExampleCorp team are mainly “DevOps” but there’s separation of duties between the cloud infrastructure team and the application team. Application code is pushed via CI/CD pipeline around once a day; the platform engineering team tests and deploys infrastructure changes. The operating model focuses on fast deployment of new features, and rapid feedback on user experience. Application performance and availability are vital to accelerated sign ups, which drive our funding valuation. 
 
__*Our Team:*__ 
 
| Title | Name |   Requirements |
|-------|------|----------------|
| CEO | Ada Lovelace | focused on growing user base to secure funding, building a sustainable revenue stream. |
| CIO | Bertrand Russell | focused on efficiency and performance goals for IT Operations, release targets and application availability. | 
| Infrastructure Director | Alan Turing | Primary concern is security and stability of infrastructure. | 
| Application Director | Grace Hopper | making sure production is delighting customers and features are the right features are being delivered to production as quickly as possible | 
| InfoSec Director | Elizabeth Feinler | security threats posed by fast delivery, competing priorities between security and release timelines. | 
| DevOps Engineer | Blaise Pascal | wants insight into platform changes, application issues, and governance requirements affecting feature delivery and application security. | 
| Site Reliability Engineer | Charles Babbage | wants to stopped getting paged at all hours and wants to better understand system reliability 
| Security Engineer | Edsger Dijkstra | Biggest challenge: speed to market sometimes means security issues come up only post-go-live. 
| User Experience Lead | Cindy Logan-Matthew | Biggest challenge: user feature requests growing at a rate that exceeding ExampleCorp’ ability to implement them. | 
| Financial Analyst | George Boole | Biggest challenge is variable operating expenses and rapid rate of utilization expansion make cost control difficult. |
 
**2.2 Building the Monitoring Plan** 
 
There are two goals of monitoring: 
- Achieve situational awareness to provide timely and effective responses and 
- Gain insights for the business, development, and operations that enable proactive courses of action. 
 
Understanding needs requires engagement. Users frequently ask for what they think they need or what they understand is possible. Apply the “5 Whys” technique, and do not assume that what they are asking for is what they actually need. Operations' engagement with teams enables understanding what they are trying to achieve and help determine options for achieving their monitoring goals. 
 
Consider: 
 
- Personas and requirements: Who will consume the outputs from monitoring? What are their goals? 
- System architecture and insights: What are the components? What telemetry should be collected? 
- Thresholds for testing: What does good look like? How will it be measured? 
- Reporting: What information should be provided via "pull" on-demand in Dashboards? Who are the stakeholders? 
- Alerts and actions: What are the alerts that align to business outcomes? What actions should be taken when alerts occur? 

 
