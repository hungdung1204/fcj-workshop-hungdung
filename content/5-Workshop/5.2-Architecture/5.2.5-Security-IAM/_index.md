---
title: "Security and IAM"
date: 2026-07-03
weight: 5
chapter: false
pre: " <b> 5.2.5. </b> "
---

## Security principles

The system applies the following security principles:

- AWS WAF protects the CloudFront distribution at the edge.
- Backend services run in private subnets.
- ECS tasks do not have public IP addresses.
- RDS is placed in private DB subnets.
- Only the ALB receives API traffic from CloudFront.
- Security groups restrict traffic to required sources only.
- No access keys are hard-coded in source code.
- Secrets are stored in AWS Secrets Manager.
- IAM roles are granted per component.
- Protected APIs require JWT tokens from Cognito.
- VPC Endpoints are used for private traffic to AWS managed services.

## AWS WAF security layer

AWS WAF is placed in front of CloudFront to filter requests before they reach the frontend or API backend. The Web ACL uses AWS Managed Rules to reduce the need to write custom security rules manually.

Main rule groups include:

- **AWSManagedRulesCommonRuleSet:** Protects against common suspicious request patterns.
- **AWSManagedRulesKnownBadInputsRuleSet:** Blocks known malicious inputs.
- **AWSManagedRulesAmazonIpReputationList:** Blocks or controls requests from low-reputation IP addresses.
- **RateLimitPerIp:** Limits request volume from a single IP address within a period of time.

## IAM roles

AWS components use IAM roles instead of static access keys:

- **Lambda execution role:** Allows Lambda to write logs and access required resources.
- **ECS task execution role:** Allows ECS to pull images from ECR and write logs to CloudWatch.
- **ECS task role:** Allows the application to read secrets, publish to SNS, read/write SQS, or send emails through SES depending on the service.
- **CloudFormation execution permissions:** Allows infrastructure resources to be created and managed through templates.

This approach follows the **Principle of Least Privilege**, where each service receives only the permissions needed for its own responsibilities.

## Authentication and authorization

Authentication is handled by Amazon Cognito. Cognito verifies users and issues JWT tokens.

Authorization is handled by backend services. Services verify JWT tokens before processing requests. For admin APIs, the backend checks the `cognito:groups` claim in the token to determine whether the user belongs to the Admin group.
