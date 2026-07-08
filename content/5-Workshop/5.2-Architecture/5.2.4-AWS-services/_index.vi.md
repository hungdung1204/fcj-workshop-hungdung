---
title: "AWS services and selection rationale"
date: 2026-07-03
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

| Dịch vụ | Vai trò | Lý do lựa chọn |
|---|---|---|
| AWS WAF | Bảo vệ CloudFront distribution ở tầng edge. | Phù hợp với website public-facing, hỗ trợ AWS Managed Rules và rate-based rule. |
| Amazon S3 | Lưu frontend static files và hình ảnh sản phẩm. | Chi phí thấp, dễ triển khai static hosting, phù hợp lưu object như ảnh sản phẩm. |
| Amazon CloudFront | CDN cho frontend, ảnh sản phẩm và API entry point. | Giảm độ trễ, hỗ trợ HTTPS, cache nội dung tĩnh và route `/api/*` đến ALB. |
| CloudFront Function | Rewrite clean route frontend. | Nhẹ, chạy ở edge, giúp URL thân thiện mà không cần backend server cho route. |
| Amazon Cognito | Đăng ký, xác nhận tài khoản, đăng nhập và JWT token. | Managed authentication, tránh tự xây password hashing, token issuing và email verification. |
| AWS Lambda | Xử lý Cognito PostConfirmation trigger. | Phù hợp tác vụ nhỏ, event-driven, không cần chạy liên tục. |
| Application Load Balancer | Nhận API request và route đến ECS service. | Hỗ trợ path-based routing cho kiến trúc microservices. |
| Amazon ECS Fargate | Chạy API service và worker service dạng container. | Không cần quản lý EC2, dễ scale service, phù hợp Dockerized microservices. |
| Amazon ECR | Lưu Docker image của các service. | Tích hợp trực tiếp với ECS và hỗ trợ private container registry. |
| ECS Service Connect / AWS Cloud Map | Service discovery nội bộ. | Tránh hard-code IP task, hỗ trợ DNS nội bộ khi task restart hoặc scale. |
| Amazon RDS MySQL | Lưu dữ liệu quan hệ. | Managed relational database, hỗ trợ backup, monitoring, replication. |
| Amazon SNS / Amazon SQS | Xử lý event-driven workflow. | Tách producer và consumer, giảm coupling, giúp worker xử lý bất đồng bộ. |
| Amazon SES | Gửi email thông báo. | Managed email service, chi phí thấp cho email tự động. |
| AWS Secrets Manager | Lưu database credentials, Cognito config, internal API key. | Tránh hard-code secret trong source code hoặc Docker image. |
| Amazon CloudWatch Logs | Thu thập log API service, worker service và Lambda. | Hỗ trợ debug, kiểm thử, vận hành và theo dõi lỗi. |
| Amazon VPC / VPC Endpoints | Cô lập network và truy cập AWS service qua private path. | Backend nằm trong private subnet, giảm phụ thuộc NAT Gateway cho AWS managed services. |
| AWS CloudFormation | Triển khai hạ tầng bằng Infrastructure as Code. | Giảm lỗi thao tác thủ công, dễ triển khai lại và clean-up bằng stack. |

Trong hệ thống HaShop, các dịch vụ được chọn theo hướng ưu tiên managed service, khả năng mở rộng, giảm công sức vận hành và phù hợp với mục tiêu học tập của workshop.
