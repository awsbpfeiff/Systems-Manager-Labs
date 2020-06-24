---
weight: 3
title: "Advance Labs"
---

### IAC Code Pipeline to manage Service Catalog across multiple accounts and regions
Give administrators the ability to manage Infrastructure as code through deops pipeline which 
validates and deploys templates to Service Catalog.  This is the prefered way to manage large deployments of Service catalog
across multiple accounts using a hub and spoke deployment.  See the huba and spoke blog post [here](https://aws.amazon.com/blogs/mt/how-to-set-up-a-multi-region-multi-account-catalog-of-company-standard-aws-service-catalog-products/).  
This can be used in conjunction with Control Tower.  A dedicated lab for CT 
deployments can be found [here](https://controltower.aws-management.tools/infrastructure/resource/sc-multiaccount/)

[CodePipeline Reference Architecture](https://github.com/aws-samples/aws-service-catalog-reference-architectures/tree/master/codepipeline)



### DevOps pipeline portfolio for ECS containers
Give end users such as developers and data engineers the ability to quickly provision products for the entire Software Development Lifecycle (SDLC).
Products include a codecommit repo and codepipeline for deploying products as container images to ECR, an ECS service to run the image, and an ECS cluster to host the services.


[DevOps pipeline for ECS](https://github.com/aws-samples/aws-service-catalog-reference-architectures/tree/master/ecs)


