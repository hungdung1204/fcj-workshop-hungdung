---
title: "Overview"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## HaShop E-commerce Microservices Platform on AWS

HaShop is an online clothing e-commerce system built on Amazon Web Services. The system simulates a real e-commerce website where users can register an account, sign in, browse products, select size/color, add products to cart, place orders, use simulated payment methods, and receive email notifications.

Traditional e-commerce systems are often implemented as monolithic applications. This model is simple at the beginning, but as the system grows it becomes harder to maintain, harder to scale by feature, harder to deploy independently, and more likely to affect the whole system when one module fails.

For that reason, HaShop uses **Microservices** combined with **Event-Driven Architecture** on AWS. Core business functions are separated into independent services such as User Service, Product Service, Inventory Service, Cart Service, Order Service, Payment Service, and Notification Service. These services are packaged with Docker and deployed on Amazon ECS Fargate in private subnets.

The system uses Amazon CloudFront to distribute the frontend and forward API requests, Amazon Cognito for user authentication, Amazon RDS MySQL for data storage, Amazon SNS/SQS for asynchronous processing, and AWS WAF to strengthen edge protection against malicious requests.

### 5.1.1. Context and problem statement

The project focuses on the following problems:

- Build an e-commerce system with an architecture close to a real-world cloud application.
- Split business capabilities into independent microservices.
- Deploy containerized backends on AWS without manually managing EC2 instances.
- Authenticate users securely with a managed service.
- Process payment and email notification tasks asynchronously.
- Protect the frontend/API entry point from malicious requests.
- Redeploy the infrastructure through Infrastructure as Code.
- Optimize security, logging, monitoring, and resource clean-up.

### 5.1.2. Target users

| User group | Main needs |
|---|---|
| Customer | Register, confirm account, sign in, browse products, choose size/color, add products to cart, place orders, and track order status. |
| Admin | Manage users, categories, sizes, colors, products, product images, inventory, orders, and order processing status. |

Within the workshop scope, HaShop is used for learning, demonstrating cloud-native architecture, and practicing end-to-end deployment on AWS.

### 5.1.3. Project goals

The goal of this workshop is to build and deploy a microservices e-commerce platform on AWS using managed services to reduce operational effort, improve scalability, and strengthen security.

The system demonstrates the main components of a cloud-native application:

- Frontend hosting
- Edge security
- Authentication and authorization
- API routing
- Containerized backend services
- Service-to-service communication
- Relational database
- Object storage
- Event-driven processing
- Logging and monitoring
- Infrastructure as Code
- Cost optimization and clean-up

### 5.1.4. Expected outputs

After completing the workshop, the learner should be able to deploy a working HaShop system on AWS with the following outputs:

- A frontend website accessible through a CloudFront domain.
- AWS WAF Web ACL associated with the CloudFront distribution.
- Backend API services running on ECS Fargate.
- Users can register, confirm accounts, and sign in with Cognito.
- Admin users can manage products, categories, sizes, colors, and orders.
- Customers can add products to cart and place orders.
- Payment Worker processes simulated payment through SQS.
- Order Worker updates order status after payment results are available.
- Notification Worker sends email notifications through SES.
- API service, worker service, and Lambda logs are recorded in CloudWatch Logs.
- The main infrastructure is deployed with CloudFormation.

### 5.1.5. Success criteria

The project is considered successful when it meets the following criteria:

- CloudFormation stacks deploy successfully.
- Frontend files are uploaded to S3 and accessible through CloudFront.
- AWS WAF is associated with the CloudFront distribution.
- ECS services run steadily with `1/1 tasks running`.
- ALB routes requests to the correct backend services.
- Users can register, confirm accounts, and sign in.
- Backend services verify JWT tokens before processing protected requests.
- Admin users can add products and manage orders.
- Customers can check out COD and MOMO/BANK demo orders.
- Messages flow through SNS/SQS and are processed by workers.
- CloudWatch Logs records logs from API services and worker services.
- A clean-up guide exists to prevent unnecessary AWS charges.
