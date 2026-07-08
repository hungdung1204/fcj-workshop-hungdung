---
title: "Main request flows"
date: 2026-07-03
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

## Luồng người dùng truy cập frontend

```text
User -> AWS WAF -> Amazon CloudFront -> S3 Frontend Bucket
```

## Luồng người dùng gọi API

```text
User -> AWS WAF -> Amazon CloudFront -> Application Load Balancer
     -> ECS Fargate API Service -> Amazon RDS / S3 / SNS / SQS
```

## Luồng xác thực

```text
Frontend -> Amazon Cognito -> JWT token
Frontend -> Authorization header -> Backend service verify JWT -> Process request
```

Sau khi đăng nhập thành công, Cognito trả về Access Token, ID Token và Refresh Token. Frontend gửi Access Token trong header `Authorization: Bearer <accessToken>` khi gọi các API cần xác thực.

## Luồng thanh toán bất đồng bộ

```text
order-api -> SNS Payment Requested Topic -> SQS Payment Requested Queue
          -> payment-worker -> SNS Payment Result Topic
          -> SQS Payment Result Queue -> order-worker -> update order status
```

## Luồng notification

```text
order-api / order-worker -> SNS Notification Topic -> SQS Notification Queue
                         -> notification-worker -> Amazon SES -> User/Admin email
```

Thiết kế này giúp Order Service không cần gọi trực tiếp Payment Worker hoặc Notification Worker. Các service chỉ cần publish event, còn worker xử lý bất đồng bộ theo message trong queue.
