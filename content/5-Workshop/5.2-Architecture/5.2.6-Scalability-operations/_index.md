---
title: "Scalability and operations"
date: 2026-07-03
weight: 6
chapter: false
pre: " <b> 5.2.6. </b> "
---

## Scalability

HaShop can scale across multiple layers:

- AWS WAF protects requests at the edge.
- CloudFront caches static content and reduces load on origins.
- ECS Fargate services can increase task count when traffic grows.
- ALB distributes requests across backend tasks.
- SNS/SQS decouples producers and consumers for asynchronous processing.
- Worker services can scale independently based on queue depth.
- RDS can be upgraded, extended with read replicas, or split by service in production.

In the demo environment, each service runs one task to optimize cost. In a real environment, ECS Service Auto Scaling can be enabled based on CPU, memory, or SQS queue depth.

## Logging and monitoring

CloudWatch Logs is used to monitor logs for each service. Each ECS service has its own log group, making debugging easier.

Important metrics to monitor include:

- AWS WAF allowed requests / blocked requests / rate limit rule matches.
- CloudFront requests and CloudFront 4xx/5xx error rate.
- ECS CPU utilization and memory utilization.
- ALB target response time and HTTP 4xx/5xx count.
- RDS CPU utilization and database connections.
- SQS approximate number of messages visible and approximate age of oldest message.
- CloudWatch log errors.

## Alerts

The system can configure CloudWatch Alarms for the following cases:

- AWS WAF blocked requests increase abnormally.
- CloudFront 5xx error rate is high.
- ECS service tasks stop or desired count is not reached.
- ALB returns many HTTP 5xx responses.
- RDS CPU is high.
- SQS queue backlog grows.
- Workers do not process messages for a long time.
- NAT Gateway or VPC Endpoint traffic becomes abnormal.

These alarms can publish notifications to an SNS Topic so administrators can receive email alerts.
