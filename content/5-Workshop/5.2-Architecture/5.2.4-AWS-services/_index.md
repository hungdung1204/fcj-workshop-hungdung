---
title: "AWS services and selection rationale"
date: 2026-07-03
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

| Service | Role | Why it is selected |
|---|---|---|
| AWS WAF | Protects the CloudFront distribution at the edge. | Suitable for a public-facing website, supports AWS Managed Rules and rate-based rules. |
| Amazon S3 | Stores static frontend files and product images. | Low cost, simple for static hosting, suitable for object storage such as product images. |
| Amazon CloudFront | CDN for frontend, product images, and API entry point. | Reduces latency, supports HTTPS, caches static content, and routes `/api/*` to ALB. |
| CloudFront Function | Rewrites frontend clean routes. | Lightweight edge function that provides friendly URLs without a backend route server. |
| Amazon Cognito | Handles registration, account confirmation, sign-in, and JWT tokens. | Managed authentication avoids building password hashing, token issuing, and email verification manually. |
| AWS Lambda | Handles Cognito PostConfirmation trigger. | Suitable for small event-driven tasks that do not need to run continuously. |
| Application Load Balancer | Receives API requests and routes them to ECS services. | Supports path-based routing for microservices. |
| Amazon ECS Fargate | Runs API services and worker services as containers. | No EC2 management, service-level scaling, and suitable for Dockerized microservices. |
| Amazon ECR | Stores Docker images for services. | Integrates directly with ECS and supports private container registry. |
| ECS Service Connect / AWS Cloud Map | Provides internal service discovery. | Avoids hard-coded task IPs and supports internal DNS when tasks restart or scale. |
| Amazon RDS MySQL | Stores relational data. | Managed relational database with backup, monitoring, and replication support. |
| Amazon SNS / Amazon SQS | Implements event-driven workflows. | Decouples producers and consumers and enables asynchronous worker processing. |
| Amazon SES | Sends email notifications. | Managed email service with low cost for automated emails. |
| AWS Secrets Manager | Stores database credentials, Cognito config, and internal API keys. | Avoids hard-coded secrets in source code or Docker images. |
| Amazon CloudWatch Logs | Collects logs from API services, workers, and Lambda. | Supports debugging, validation, operations, and error tracking. |
| Amazon VPC / VPC Endpoints | Isolates networking and provides private access to AWS services. | Keeps backends in private subnets and reduces NAT Gateway dependency for AWS managed services. |
| AWS CloudFormation | Deploys infrastructure as code. | Reduces manual errors, supports repeatable deployment, and simplifies clean-up through stacks. |

In HaShop, services are selected with a preference for managed services, scalability, reduced operational effort, and alignment with the learning goals of the workshop.
