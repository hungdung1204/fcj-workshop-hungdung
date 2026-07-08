---
title: "Architecture diagram"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

![HaShop AWS Microservices Architecture](/images/5-Workshop/hashop-theory/hashop-architecture.png)

*Figure 1. HaShop AWS Microservices Architecture with CloudFront, AWS WAF, ECS Fargate, RDS, SNS/SQS and VPC Endpoints.*

Sơ đồ thể hiện luồng truy cập từ người dùng đến AWS WAF, CloudFront, S3 frontend, ALB, ECS Fargate services, Amazon Cognito, RDS MySQL, SNS/SQS workers, SES, CloudWatch Logs, Secrets Manager và VPC Endpoints.

Các thành phần quan trọng trong sơ đồ:

- **Edge/Global:** AWS WAF, CloudFront và CloudFront Function.
- **Frontend:** S3 Frontend Bucket lưu static HTML/CSS/JavaScript.
- **Authentication:** Amazon Cognito và Lambda PostConfirmation.
- **Application:** ALB, ECS Fargate API services và worker services.
- **Data:** Amazon RDS MySQL primary/standby hoặc read replica.
- **Async layer:** Amazon SNS, Amazon SQS và Amazon SES.
- **Operations:** AWS Secrets Manager, Amazon CloudWatch Logs và VPC Endpoints.
