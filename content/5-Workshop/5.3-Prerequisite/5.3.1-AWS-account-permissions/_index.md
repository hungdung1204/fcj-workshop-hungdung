---
title: "AWS account and permissions"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

The deployment requires an AWS account that can create and manage the main services used by HaShop:

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
- IAM Role and IAM Policy

For a lab environment, `AdministratorAccess` can be used to avoid permission errors while practicing. For a real environment, create IAM roles and policies based on the principle of least privilege.
