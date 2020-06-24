---
weight: 7
title: AppConfig
description: AppConfig Immersion Day Lab
---

### Introduction

In this workshop, we will learn how to:
 
1. Create an AppConfig configuration, store it in S3 and deploy it. 

2. Call the AppConfig API from API Gateway (Lambda) to get your AppConfig Configuration Document.
 
3. Use the AppConfig Configuration Document returned from AppConfig API to turn on a feature and limit the number of results returned by API Gateway.

4. Validate the AppConfig Configuration Document with a JSON Schema validator as a means to perform unit testing.

5. Validate the AppConfig Configuration Document with a Lambda validator as a means to perform integration testing.

6. Create a CloudWatch Alarm to monitor the API Gateway Lambda for errors during AppConfig deployment as a trigger to rollback deployment.