---
title: "Tài khoản AWS và quyền cần có"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

Quá trình triển khai cần một tài khoản AWS có quyền tạo và quản lý các dịch vụ chính của HaShop:

- AWS CloudFormation
- AWS WAF
- Amazon S3
- Amazon CloudFront
- Amazon ECR
- Amazon ECS Fargate
- Application Load Balancer
- Amazon VPC
- Amazon RDS MySQL
- Amazon Cognito
- AWS Lambda
- Amazon SNS
- Amazon SQS
- Amazon SES
- AWS Secrets Manager
- Amazon CloudWatch Logs
- IAM Role và IAM Policy

Trong môi trường lab, có thể dùng quyền `AdministratorAccess` để giảm lỗi thiếu quyền khi thực hành. Trong môi trường thực tế, nên tạo IAM Role và IAM Policy theo nguyên tắc least privilege.
