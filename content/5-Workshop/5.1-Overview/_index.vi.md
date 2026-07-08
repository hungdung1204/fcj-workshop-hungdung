---
title: "Overview"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## HaShop E-commerce Microservices Platform on AWS

HaShop là hệ thống thương mại điện tử bán quần áo trực tuyến được xây dựng trên Amazon Web Services. Hệ thống mô phỏng một website e-commerce thực tế, trong đó người dùng có thể đăng ký tài khoản, đăng nhập, xem sản phẩm, chọn size/màu sắc, thêm sản phẩm vào giỏ hàng, đặt hàng, thanh toán mô phỏng và nhận thông báo qua email.

Trong các hệ thống thương mại điện tử truyền thống, toàn bộ chức năng thường được xây dựng trong một ứng dụng monolithic. Mô hình này đơn giản ở giai đoạn đầu, nhưng khi hệ thống mở rộng sẽ gặp nhiều hạn chế như khó bảo trì, khó mở rộng riêng từng chức năng, khó triển khai độc lập và dễ ảnh hưởng toàn bộ hệ thống khi một module gặp lỗi.

Vì vậy, dự án HaShop lựa chọn kiến trúc **Microservices** kết hợp **Event-Driven Architecture** trên AWS. Các chức năng chính được tách thành nhiều service độc lập như User Service, Product Service, Inventory Service, Cart Service, Order Service, Payment Service và Notification Service. Các service được đóng gói bằng Docker và triển khai trên Amazon ECS Fargate trong private subnet.

Hệ thống sử dụng Amazon CloudFront để phân phối frontend và forward API request, Amazon Cognito để xác thực người dùng, Amazon RDS MySQL để lưu trữ dữ liệu, Amazon SNS/SQS để xử lý bất đồng bộ và AWS WAF để tăng cường bảo vệ tầng edge trước các request độc hại.

### 5.1.1. Bối cảnh và bài toán

Dự án tập trung giải quyết các vấn đề sau:

- Xây dựng một hệ thống e-commerce có kiến trúc gần với thực tế.
- Tách các chức năng thành nhiều microservice độc lập.
- Triển khai backend container trên AWS mà không cần quản lý EC2 thủ công.
- Xác thực người dùng an toàn bằng managed service.
- Xử lý bất đồng bộ các tác vụ như thanh toán và gửi email.
- Bảo vệ frontend/API entry point trước các request độc hại.
- Triển khai lại toàn bộ hạ tầng bằng Infrastructure as Code.
- Tối ưu bảo mật, logging, monitoring và clean-up chi phí.

### 5.1.2. Người dùng mục tiêu

| Nhóm người dùng | Nhu cầu chính |
|---|---|
| Customer | Đăng ký, xác nhận tài khoản, đăng nhập, xem sản phẩm, chọn size/màu sắc, thêm sản phẩm vào giỏ hàng, đặt hàng và theo dõi trạng thái đơn hàng. |
| Admin | Quản lý người dùng, danh mục, size, màu sắc, sản phẩm, hình ảnh sản phẩm, tồn kho, đơn hàng và trạng thái xử lý đơn hàng. |

Trong phạm vi workshop, HaShop phục vụ mục tiêu học tập, trình diễn kiến trúc cloud-native và triển khai end-to-end trên AWS.

### 5.1.3. Mục tiêu dự án

Mục tiêu của workshop là xây dựng và triển khai một nền tảng thương mại điện tử dạng microservices trên AWS, sử dụng các dịch vụ managed service để giảm công sức vận hành, tăng khả năng mở rộng và cải thiện bảo mật.

Hệ thống cần thể hiện được các thành phần chính của một ứng dụng cloud-native:

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

### 5.1.4. Output mong muốn

Sau khi hoàn thành workshop, người thực hiện có thể triển khai được một hệ thống HaShop hoàn chỉnh trên AWS với các output sau:

- Website frontend truy cập qua CloudFront domain.
- AWS WAF Web ACL được gắn với CloudFront distribution.
- Backend API service chạy trên ECS Fargate.
- Người dùng có thể đăng ký, xác nhận tài khoản và đăng nhập bằng Cognito.
- Admin có thể quản lý sản phẩm, danh mục, size, màu sắc và đơn hàng.
- Customer có thể thêm sản phẩm vào giỏ hàng và đặt hàng.
- Payment Worker xử lý thanh toán mô phỏng qua SQS.
- Order Worker cập nhật trạng thái đơn hàng sau khi có kết quả thanh toán.
- Notification Worker gửi email thông báo qua SES.
- Log của API service, worker service và Lambda được ghi nhận trong CloudWatch Logs.
- Toàn bộ hạ tầng chính được triển khai bằng CloudFormation.

### 5.1.5. Tiêu chí đánh giá thành công

Dự án được xem là thành công khi đáp ứng các tiêu chí sau:

- CloudFormation stack deploy thành công.
- Frontend được upload lên S3 và truy cập được qua CloudFront.
- AWS WAF được associate với CloudFront distribution.
- ECS services chạy ổn định với trạng thái `1/1 tasks running`.
- ALB route đúng request đến từng backend service.
- Người dùng có thể đăng ký, xác nhận tài khoản và đăng nhập.
- JWT token được backend verify trước khi xử lý request bảo mật.
- Admin có thể thêm sản phẩm và quản lý đơn hàng.
- Customer có thể checkout đơn hàng COD và MOMO/BANK demo.
- Message đi qua SNS/SQS và được worker xử lý.
- CloudWatch Logs ghi nhận log của API service và worker service.
- Có hướng dẫn clean-up để tránh phát sinh chi phí.
