---
title: "Main request flows"
date: 2026-07-03
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

## Frontend access flow

```text
User -> AWS WAF -> Amazon CloudFront -> S3 Frontend Bucket
```

## API request flow

```text
User -> AWS WAF -> Amazon CloudFront -> Application Load Balancer
     -> ECS Fargate API Service -> Amazon RDS / S3 / SNS / SQS
```

## Authentication flow

```text
Frontend -> Amazon Cognito -> JWT token
Frontend -> Authorization header -> Backend service verify JWT -> Process request
```

After a successful sign-in, Cognito returns Access Token, ID Token, and Refresh Token. The frontend sends the Access Token in the `Authorization: Bearer <accessToken>` header when calling protected APIs.

## Asynchronous payment flow

```text
order-api -> SNS Payment Requested Topic -> SQS Payment Requested Queue
          -> payment-worker -> SNS Payment Result Topic
          -> SQS Payment Result Queue -> order-worker -> update order status
```

## Notification flow

```text
order-api / order-worker -> SNS Notification Topic -> SQS Notification Queue
                         -> notification-worker -> Amazon SES -> User/Admin email
```

This design allows the Order Service to avoid directly calling the Payment Worker or Notification Worker. Services only publish events, while workers process messages asynchronously from queues.
