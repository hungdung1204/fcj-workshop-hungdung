---
title: "Scalability and operations"
date: 2026-07-03
weight: 6
chapter: false
pre: " <b> 5.2.6. </b> "
---

## Khả năng mở rộng

Kiến trúc HaShop có khả năng mở rộng theo nhiều lớp:

- AWS WAF xử lý bảo vệ request ở edge.
- CloudFront cache static content và giảm tải cho origin.
- ECS Fargate service có thể tăng số lượng task khi traffic tăng.
- ALB phân phối request đến nhiều task backend.
- SNS/SQS tách producer và consumer để xử lý bất đồng bộ.
- Worker service có thể scale độc lập theo số lượng message trong queue.
- RDS có thể nâng cấp instance class, sử dụng read replica hoặc tách database theo từng service trong production.

Trong phạm vi demo, mỗi service chạy một task để tối ưu chi phí. Trong môi trường thực tế, có thể bật ECS Service Auto Scaling dựa trên CPU, memory hoặc queue depth của SQS.

## Logging và Monitoring

CloudWatch Logs được sử dụng để theo dõi log của từng service. Mỗi ECS service có log group riêng, giúp việc debug dễ hơn.

Các chỉ số quan trọng có thể theo dõi gồm:

- AWS WAF allowed requests / blocked requests / rate limit rule matches.
- CloudFront requests và CloudFront 4xx/5xx error rate.
- ECS CPU utilization và memory utilization.
- ALB target response time và HTTP 4xx/5xx count.
- RDS CPU utilization và database connections.
- SQS approximate number of messages visible và approximate age of oldest message.
- CloudWatch log errors.

## Alert

Hệ thống có thể cấu hình CloudWatch Alarm cho các trường hợp:

- AWS WAF block request tăng bất thường.
- CloudFront 5xx error rate cao.
- ECS service task bị stop hoặc desired count không đạt.
- ALB có nhiều HTTP 5xx.
- RDS CPU cao.
- SQS queue tồn đọng nhiều message.
- Worker không xử lý message trong thời gian dài.
- NAT Gateway hoặc VPC Endpoint phát sinh traffic bất thường.

Các alarm này có thể gửi thông báo đến SNS Topic để admin nhận email cảnh báo.
