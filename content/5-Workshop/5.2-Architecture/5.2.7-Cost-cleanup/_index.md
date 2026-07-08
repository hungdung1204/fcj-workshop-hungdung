---
title: "Cost optimization and clean-up"
date: 2026-07-03
weight: 7
chapter: false
pre: " <b> 5.2.7. </b> "
---

## Cost optimization

Because this is a learning and demo environment, cost control is important. Recommended optimization actions include:

- Run each ECS service with `DesiredCount = 1` in the demo environment.
- Use small RDS instances.
- Combine multiple database schemas into one primary RDS instance for the demo.
- Turn on the environment only when testing or demoing.
- Delete stacks after completion.
- Delete unused ECR images and S3 artifacts.
- Configure log retention to prevent CloudWatch Logs cost growth.
- Monitor NAT Gateway and VPC Endpoints because they have hourly charges.
- Monitor AWS WAF requests and rules to avoid unnecessary rule configuration.

In production, databases can be separated by service for stronger isolation. In the demo environment, combining schemas into one RDS instance significantly reduces cost.

## Clean-up

Resources with notable costs include AWS WAF Web ACL, ECS Fargate tasks, RDS primary instance and read replica, Application Load Balancer, NAT Gateway, VPC Interface Endpoints, CloudFront distribution, S3 buckets, ECR repositories, CloudWatch Logs, and Secrets Manager secrets.

Recommended clean-up order:

1. Delete the ECS services stack.
2. Delete the DB init stack if it still exists.
3. Delete the foundation stack.
4. Delete the global WAF stack.
5. Delete ECR repositories if they were created outside CloudFormation.
6. Delete the S3 artifact bucket if it is no longer needed.
7. Check the Billing Dashboard to confirm that no remaining resources continue to generate cost.

Note: CloudFront distributions may take time to disable and delete completely. S3 buckets must also be emptied before deletion if objects still exist inside them.
