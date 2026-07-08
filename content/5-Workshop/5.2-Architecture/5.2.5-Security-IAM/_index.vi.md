---
title: "Security and IAM"
date: 2026-07-03
weight: 5
chapter: false
pre: " <b> 5.2.5. </b> "
---

## Nguyên tắc bảo mật

Hệ thống áp dụng các nguyên tắc bảo mật cơ bản sau:

- AWS WAF bảo vệ CloudFront distribution ở tầng edge.
- Backend service chạy trong private subnet.
- ECS task không có public IP.
- RDS nằm trong private DB subnet.
- Chỉ ALB nhận API traffic từ CloudFront.
- Security group giới hạn traffic theo đúng nguồn cần thiết.
- Không hard-code access key trong source code.
- Secret được lưu trong AWS Secrets Manager.
- IAM Role được cấp quyền theo từng thành phần.
- API bảo mật yêu cầu JWT token từ Cognito.
- VPC Endpoints được sử dụng cho traffic private đến các AWS managed services.

## AWS WAF Security Layer

AWS WAF được đặt trước CloudFront để lọc request trước khi request đến frontend hoặc API backend. WAF Web ACL sử dụng các AWS Managed Rules nhằm giảm công sức tự viết rule thủ công.

Các nhóm rule chính gồm:

- **AWSManagedRulesCommonRuleSet:** Bảo vệ trước các request phổ biến có dấu hiệu bất thường.
- **AWSManagedRulesKnownBadInputsRuleSet:** Chặn các input đã biết là độc hại.
- **AWSManagedRulesAmazonIpReputationList:** Chặn hoặc kiểm soát request từ các IP có độ uy tín thấp.
- **RateLimitPerIp:** Giới hạn số request từ một IP trong một khoảng thời gian.

## IAM Role

Các thành phần AWS sử dụng IAM role thay vì access key tĩnh:

- **Lambda execution role:** Cho phép Lambda ghi log và truy cập tài nguyên cần thiết.
- **ECS task execution role:** Cho phép ECS pull image từ ECR và ghi log vào CloudWatch.
- **ECS task role:** Cho phép application đọc secret, publish SNS, đọc/ghi SQS hoặc gửi email qua SES tùy service.
- **CloudFormation execution permissions:** Cho phép tạo và quản lý tài nguyên theo template.

Cách tiếp cận này phù hợp với nguyên tắc **Principle of Least Privilege**, tức là mỗi service chỉ nên có quyền cần thiết cho nhiệm vụ của nó.

## Authentication và Authorization

Authentication được thực hiện bằng Amazon Cognito. Cognito chịu trách nhiệm xác thực người dùng và phát hành JWT token.

Authorization được xử lý ở backend service. Các service kiểm tra JWT token trước khi xử lý request. Với các API admin, backend kiểm tra claim `cognito:groups` trong token để xác định người dùng có thuộc nhóm Admin hay không.
