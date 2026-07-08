---
title: "Cost optimization and clean-up"
date: 2026-07-03
weight: 7
chapter: false
pre: " <b> 5.2.7. </b> "
---

## Tối ưu chi phí

Do đây là môi trường học tập và demo, chi phí là yếu tố quan trọng. Một số hướng tối ưu gồm:

- Chạy mỗi ECS service với `DesiredCount = 1` trong môi trường demo.
- Sử dụng RDS instance nhỏ.
- Gom nhiều database schema vào một primary RDS instance trong demo.
- Chỉ bật môi trường khi cần test/demo.
- Xóa stack sau khi hoàn thành.
- Xóa ECR image và S3 artifact không còn sử dụng.
- Cấu hình log retention để tránh CloudWatch Logs tăng chi phí lâu dài.
- Theo dõi NAT Gateway và VPC Endpoint vì đây là các thành phần có chi phí hourly.
- Theo dõi AWS WAF request và rule để tránh cấu hình rule không cần thiết.

Trong môi trường production, có thể tách database theo từng service để tăng tính cô lập. Tuy nhiên, trong môi trường demo, việc gom nhiều schema vào một RDS instance giúp giảm chi phí đáng kể.

## Clean-up

Các tài nguyên có chi phí đáng chú ý gồm AWS WAF Web ACL, ECS Fargate tasks, RDS primary instance và read replica, Application Load Balancer, NAT Gateway, VPC Interface Endpoints, CloudFront distribution, S3 bucket, ECR repository, CloudWatch Logs và Secrets Manager secrets.

Thứ tự clean-up nên thực hiện:

1. Xóa stack ECS services.
2. Xóa stack DB init nếu còn tồn tại.
3. Xóa stack foundation.
4. Xóa stack global WAF.
5. Xóa ECR repositories nếu được tạo ngoài CloudFormation.
6. Xóa S3 artifact bucket nếu không còn sử dụng.
7. Kiểm tra lại Billing Dashboard để chắc chắn không còn tài nguyên phát sinh chi phí.

Lưu ý: CloudFront distribution có thể cần thời gian để disable và delete hoàn toàn. S3 bucket cũng cần được empty trước khi xóa nếu còn object bên trong.
