---
title: "Architecture diagram"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

![HaShop AWS Microservices Architecture](/images/5-Workshop/hashop-theory/hashop-architecture.png)

*Figure 1. HaShop AWS Microservices Architecture with CloudFront, AWS WAF, ECS Fargate, RDS, SNS/SQS and VPC Endpoints.*

The diagram shows the access flow from users to AWS WAF, CloudFront, S3 frontend, ALB, ECS Fargate services, Amazon Cognito, RDS MySQL, SNS/SQS workers, SES, CloudWatch Logs, Secrets Manager, and VPC Endpoints.

Key components in the diagram:

- **Edge/Global:** AWS WAF, CloudFront, and CloudFront Function.
- **Frontend:** S3 Frontend Bucket stores static HTML/CSS/JavaScript.
- **Authentication:** Amazon Cognito and Lambda PostConfirmation.
- **Application:** ALB, ECS Fargate API services, and worker services.
- **Data:** Amazon RDS MySQL primary/standby or read replica.
- **Async layer:** Amazon SNS, Amazon SQS, and Amazon SES.
- **Operations:** AWS Secrets Manager, Amazon CloudWatch Logs, and VPC Endpoints.
