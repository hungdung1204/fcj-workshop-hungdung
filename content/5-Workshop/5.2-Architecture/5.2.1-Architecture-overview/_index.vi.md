---
title: "Architecture overview"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

HaShop sử dụng kiến trúc nhiều lớp trên AWS, bao gồm **Edge Security Layer**, **Frontend Layer**, **Authentication Layer**, **Application Layer**, **Service Discovery Layer**, **Asynchronous Processing Layer**, **Data Layer** và **Security and Operations Layer**.

Người dùng truy cập hệ thống thông qua Amazon CloudFront. Trước khi request được CloudFront xử lý, AWS WAF Web ACL được áp dụng ở tầng edge để kiểm tra và chặn các request có dấu hiệu độc hại. WAF giúp bảo vệ hệ thống khỏi các loại request phổ biến như bad input, request từ IP có độ uy tín thấp và request vượt quá rate limit.

CloudFront phân phối frontend static files từ Amazon S3. Với các request API có tiền tố `/api/*`, CloudFront chuyển tiếp request đến Application Load Balancer. ALB sử dụng path-based routing để đưa request đến đúng ECS Fargate service.

Backend của hệ thống gồm nhiều microservice chạy trên ECS Fargate trong private subnet. Các service không có public IP, chỉ nhận traffic từ ALB hoặc giao tiếp nội bộ với nhau thông qua ECS Service Connect và AWS Cloud Map.

Dữ liệu quan hệ được lưu trong Amazon RDS MySQL. Trong phạm vi demo, hệ thống sử dụng một RDS primary instance chứa nhiều database schema như `ecommerce_user_db`, `ecommerce_product_db`, `ecommerce_inventory_db`, `ecommerce_cart_db`, `ecommerce_order_db` và `ecommerce_payment_db`. Một RDS read replica được sử dụng để minh họa replication và khả năng dự phòng dữ liệu.

Các tác vụ bất đồng bộ như xử lý thanh toán và gửi email được thực hiện thông qua Amazon SNS và Amazon SQS. API service publish event vào SNS Topic. SNS chuyển message đến SQS Queue thông qua subscription. Các worker service chạy trên ECS Fargate long-poll message từ SQS, xử lý nghiệp vụ và xóa message khỏi queue sau khi xử lý thành công.
