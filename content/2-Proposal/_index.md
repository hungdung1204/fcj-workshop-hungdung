---
title: "Proposal"
date: 2026-07-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# HaShop E-commerce Platform
## A Microservices E-commerce Solution on AWS with Event-Driven Architecture

### 1. Executive Summary

HaShop is an online clothing e-commerce platform built on Amazon Web Services (AWS). The project applies a **Microservices** architecture combined with **Event-Driven Architecture** to create a scalable, maintainable, secure, and cloud-native system.

The platform uses **Amazon ECS Fargate** to run containerized backend services, **Amazon Cognito** for user authentication, **Amazon RDS MySQL** for business data storage, and **Amazon S3** with **Amazon CloudFront** to distribute the frontend and product images. Asynchronous tasks such as payment processing, order status updates, and email notification delivery are implemented with **Amazon SNS**, **Amazon SQS**, and ECS Fargate worker services.

The goal of the project is not only to build a functional e-commerce website, but also to practice important AWS components in a realistic architecture, including networking, security, container orchestration, service discovery, event-driven processing, observability, and infrastructure as code through AWS CloudFormation.

### 2. Problem Statement

#### Current Problems

Traditional e-commerce websites built as monolithic applications often become difficult to scale and maintain as the system grows. When user management, product management, cart, order, payment, and notification features all live inside one application, changing one feature can affect the entire system.

In addition, tasks such as payment processing, email delivery, and order status updates do not always need to run synchronously inside the user's main request. If every task is handled directly in the same request flow, the system can become slower, harder to scale, and harder to recover when a supporting component fails.

#### Proposed Solution

HaShop separates the system into independent microservices such as User Service, Product Service, Inventory Service, Cart Service, Order Service, Payment Service, and asynchronous worker services. Each service is packaged as a Docker image and deployed on Amazon ECS Fargate inside private subnets, reducing server management effort while improving security.

The frontend is deployed as a static website on Amazon S3 and distributed through Amazon CloudFront. CloudFront Function rewrites clean routes such as `/login`, `/profile`, `/cart`, and `/products/:id` to the corresponding HTML files. API requests with the `/api/*` prefix are forwarded by CloudFront to the Application Load Balancer, and the ALB routes each request to the correct backend service based on path rules.

Amazon Cognito manages user registration, account confirmation, sign-in, and JWT token issuance. Backend services verify tokens with middleware before processing protected APIs. After a user confirms an account, Cognito triggers a PostConfirmation Lambda function to synchronize the user record into Amazon RDS MySQL.

Asynchronous workflows are handled with Amazon SNS and Amazon SQS. When a user places an order using MOMO or BANK payment, the Order Service publishes a payment event to an SNS Topic. The message is fanned out to an SQS Queue, where the Payment Worker processes it. The payment result is then published through SNS/SQS so the Order Worker can update the order status and the Notification Worker can send email notifications.

#### Benefits

This solution makes the system easier to scale by business domain, reduces deployment risk, and allows each service to be tested independently. Event-Driven Architecture separates heavy or non-immediate tasks from the main request path, improving user experience and increasing system resilience.

### 3. Solution Architecture

HaShop uses a cloud-native AWS architecture with six main layers: **Edge/Frontend Layer**, **Authentication Layer**, **Application Layer**, **Async Processing Layer**, **Data Layer**, and **Management/Observability Layer**.

![HaShop AWS Architecture](/images/2-Proposal/hashop-architecture.png)

In the Edge/Frontend Layer, users access the website through Amazon CloudFront. CloudFront distributes the static frontend from S3, applies CloudFront Function for route rewriting, and forwards `/api/*` requests to the Application Load Balancer. AWS WAF is placed at the edge to improve protection against abnormal requests.

The Authentication Layer uses Amazon Cognito to manage the user pool, app client, account confirmation, and JWT token issuance. A PostConfirmation Lambda function writes newly confirmed users into the database.

The Application Layer runs on Amazon ECS Fargate. API services and worker services are placed in private subnets without public IP addresses. Services communicate internally through ECS Service Connect and an AWS Cloud Map namespace.

The Async Processing Layer uses SNS and SQS. SNS Topics publish events, while SQS Queues store messages for workers to process with long polling. Payment Worker, Order Worker, and Notification Worker process messages independently, reducing coupling between services.

The Data Layer uses Amazon RDS MySQL in private DB subnets with Multi-AZ Replication for higher availability. Data is organized by business domain, including user, product, inventory, cart, order, and payment data.

The Management/Observability Layer uses AWS Secrets Manager to store sensitive values and Amazon CloudWatch Logs to collect logs from ECS services, workers, and Lambda functions.

### 4. AWS Services Used

| Service | Role in the system |
|---|---|
| Amazon CloudFront | Distributes the frontend, forwards API requests, and serves product images. |
| CloudFront Function | Rewrites frontend clean routes to the correct HTML files. |
| AWS WAF | Protects the application at the edge from abnormal requests. |
| Amazon S3 | Stores static frontend files and product images. |
| Application Load Balancer | Receives `/api/*` requests and routes them to ECS services by path. |
| Amazon ECS Fargate | Runs backend API services and worker services as containers. |
| ECS Service Connect / AWS Cloud Map | Provides service discovery and internal communication between microservices. |
| Amazon Cognito | Manages registration, account confirmation, sign-in, and JWT tokens. |
| AWS Lambda | Handles the PostConfirmation trigger from Cognito. |
| Amazon RDS MySQL | Stores user, product, inventory, cart, order, and payment data. |
| Amazon SNS | Publishes payment and notification events. |
| Amazon SQS | Stores messages for asynchronous worker processing. |
| Amazon SES | Sends email notifications to users and administrators. |
| AWS Secrets Manager | Stores database credentials, Cognito configuration, and internal API keys. |
| Amazon CloudWatch Logs | Collects logs and supports troubleshooting. |
| AWS IAM | Grants permissions to ECS tasks, Lambda, and AWS services. |
| AWS CloudFormation | Automates infrastructure deployment. |
| VPC Endpoints | Allows ECS tasks in private subnets to access AWS services over private paths. |

### 5. Component Design

- **Frontend:** HTML/CSS/JavaScript interface stored on S3 and distributed through CloudFront.
- **User Service:** Handles registration, sign-in, user profile, and user administration; integrated with Amazon Cognito.
- **Product Service:** Manages products, categories, sizes, colors, product variants, and images.
- **Inventory Service:** Tracks inventory for each product variant.
- **Cart Service:** Manages user carts and checks product/inventory information through internal services.
- **Order Service:** Handles checkout, order status management, and payment event publishing.
- **Payment Service:** Manages user payment methods.
- **Payment Worker:** Processes asynchronous payment requests from SQS.
- **Order Worker:** Receives payment results and updates order status.
- **Notification Worker:** Receives notification events and sends emails through Amazon SES.
- **RDS MySQL:** Stores the main business data for users, products, inventory, carts, orders, and payments.

### 6. Technical Implementation

#### Implementation Phases

1. **Requirement analysis and architecture design:** Identify the main business functions of the clothing e-commerce website, choose a microservices architecture, draw the AWS architecture, and define backend services, databases, and asynchronous workflows.
2. **Backend microservices development:** Develop Node.js/Express services including User, Product, Inventory, Cart, Order, Payment, and Notification. Each service has its own responsibility, domain-specific data, and internal API communication.
3. **Authentication and authorization integration:** Integrate Amazon Cognito for registration, account confirmation, sign-in, and JWT tokens. Backend services use middleware to verify access tokens. Cognito Groups are used for Admin and Customer authorization.
4. **Event-driven workflow development:** Use SNS and SQS for asynchronous payment and notification processing. ECS Fargate worker services long-poll SQS Queues and process messages.
5. **AWS infrastructure deployment and testing:** Initially deploy the system manually through the AWS Console to validate VPC, RDS, ECS, ALB, CloudFront, SNS/SQS, Cognito, and Lambda components.
6. **Testing, debugging, and optimization:** Test the complete flow from frontend to backend, including registration, sign-in, product management, cart operations, COD checkout, MOMO/BANK checkout, worker processing, and email notifications.

#### Technical Requirements

- **Backend:** Node.js, Express.js, MySQL client, AWS SDK, Cognito JWT verifier.
- **Frontend:** HTML, CSS, JavaScript, static hosting on S3.
- **Containerization:** Docker images for each service, stored on Amazon ECR.
- **Compute:** Amazon ECS Fargate for API services and worker services.
- **Database:** Amazon RDS MySQL.
- **Authentication:** Amazon Cognito User Pool, App Client, and Cognito Groups.
- **Networking:** VPC, public subnets, private app subnets, private DB subnets, NAT Gateway, VPC Endpoints, and Security Groups.
- **Event-driven processing:** Amazon SNS, Amazon SQS, and ECS worker services.
- **Storage:** S3 Frontend Bucket and S3 Product Images Bucket.
- **Observability:** Amazon CloudWatch Logs.

### 7. Timeline & Milestones

| Phase | Main activities |
|---|---|
| Phase 1: Analysis and design | Define business requirements, design the AWS microservices architecture, and identify services, databases, and main workflows. |
| Phase 2: Backend and frontend development | Build User, Product, Inventory, Cart, Order, Payment, Notification services and the HTML/CSS/JavaScript frontend. |
| Phase 3: AWS deployment | Create VPC, subnets, security groups, RDS, Cognito, Lambda, S3, CloudFront, ECS, ALB, SNS, and SQS through the AWS Console. |
| Phase 4: Testing and optimization | Test features, fix configuration issues, add VPC Endpoints, finalize the architecture diagram, and prepare CloudFormation templates. |

### 8. Budget Estimation

The system cost depends on ECS Fargate runtime, the number of RDS instances, NAT Gateway usage, CloudFront, S3, SNS/SQS, and actual request volume. Because the project is designed for learning and demonstration, the configuration is kept as small as practical.

| Cost group | Estimated configuration | Monthly cost |
|---|---|---:|
| Amazon ECS Fargate | 9 tasks, each with 0.5 vCPU and 1 GB RAM | USD 198.43 |
| Amazon RDS MySQL | 2 db.t3.micro DB instances, Single-AZ, 20 GB storage each | USD 43.48 |
| Application Load Balancer | 1 continuously running ALB, estimated 1 LCU | USD 25.55 |
| NAT Gateway | 1 continuously running NAT Gateway, about 1 GB processed data | USD 43.13 |
| VPC Endpoints | 6 Interface Endpoints across 2 AZs; S3 Gateway Endpoint has no hourly charge | USD 122.65 |
| Amazon S3 | About 6 GB for frontend, product images, and artifacts | USD 0.16 |
| Amazon CloudFront | About 1 GB data transfer and 20,000 HTTPS requests | USD 0.00 |
| AWS WAF | 1 CloudFront Web ACL, 3 AWS Managed Rule Groups, 1 rate-based rule, approximately 20,000 requests/month | USD 9.01 |
| Amazon SNS/SQS | About 10,000 messages per month | USD 0.01 |
| Amazon Cognito | About 20 monthly active users | USD 0.00 |
| AWS Lambda | About 1,000 invocations/month, 256 MB | USD 0.00 |
| CloudWatch Logs | About 1 GB logs ingested and 1 GB stored | USD 0.53 |
| Amazon SES | About 1,000 emails per month | USD 0.10 |
| AWS Secrets Manager | 4 secrets, about 10,000 API calls/month | USD 1.65 |
| Amazon ECR | 7 Docker images, about 3.5 GB private repository storage | USD 0.35 |

**Estimated total:** approximately **USD 444.92/month**, equivalent to **USD 5,339.04 for 12 months**.

### 9. Risk Assessment

| Risk | Impact | Probability | Mitigation strategy |
|---|---|---|---|
| Networking or security group misconfiguration | High | Medium | Standardize infrastructure with CloudFormation and check target group health checks and security group rules after each deployment. |
| Cognito or JWT verification misconfiguration | High | Medium | Test registration, sign-in, token expiration, and API authorization before full integration. |
| Service Connect misconfiguration | Medium to high | Medium | Verify internal DNS resolution, Cloud Map namespace, and service logs. |
| Workers fail to process SQS messages | High | Medium | Monitor SQS, DLQ, and CloudWatch Logs for worker services. |
| AWS cost overrun | Medium | Medium | Use AWS Budget, stop unnecessary resources, and keep the environment at demo scale. |

If a service fails, the team can inspect the service-specific CloudWatch logs and update the task definition or CloudFormation template. If a worker fails to process messages, the team can inspect the SQS Queue, DLQ, and worker logs to identify the cause before replaying or reprocessing messages.

### 10. Expected Outcomes

After completion, HaShop provides a working e-commerce system with core features such as registration, sign-in, product management, inventory management, cart, checkout, simulated payment, and email notifications.

From a technical perspective, the project demonstrates how to apply Microservices and Event-Driven Architecture on AWS in a real-world scenario. The team can learn how to deploy containerized applications on ECS Fargate, use Cognito for authentication, design domain-based databases, process asynchronous tasks with SNS/SQS, and distribute a frontend using S3 and CloudFront.

In the long term, the project can be expanded into a more complete e-commerce platform with custom tailoring based on body measurements, promotion management, product reviews, real payment gateway integration, revenue analytics dashboards, and automated CI/CD.
