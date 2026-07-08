---
title: "Architecture overview"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

HaShop uses a multi-layer architecture on AWS, including **Edge Security Layer**, **Frontend Layer**, **Authentication Layer**, **Application Layer**, **Service Discovery Layer**, **Asynchronous Processing Layer**, **Data Layer**, and **Security and Operations Layer**.

Users access the system through Amazon CloudFront. Before a request is handled by CloudFront, an AWS WAF Web ACL is applied at the edge to inspect and block suspicious requests. WAF helps protect the system from common bad inputs, low-reputation IPs, and requests that exceed rate limits.

CloudFront distributes static frontend files from Amazon S3. For API requests with the `/api/*` prefix, CloudFront forwards the request to the Application Load Balancer. The ALB uses path-based routing to send each request to the correct ECS Fargate service.

The backend consists of multiple microservices running on ECS Fargate inside private subnets. These services do not have public IP addresses. They only receive traffic from the ALB or communicate internally through ECS Service Connect and AWS Cloud Map.

Relational data is stored in Amazon RDS MySQL. In the demo scope, the system uses one primary RDS instance with multiple database schemas such as `ecommerce_user_db`, `ecommerce_product_db`, `ecommerce_inventory_db`, `ecommerce_cart_db`, `ecommerce_order_db`, and `ecommerce_payment_db`. A read replica is used to demonstrate replication and data redundancy.

Asynchronous tasks such as payment processing and email notification are handled through Amazon SNS and Amazon SQS. API services publish events to SNS Topics. SNS delivers messages to SQS Queues through subscriptions. Worker services running on ECS Fargate long-poll messages from SQS, process business logic, and delete messages after successful processing.
