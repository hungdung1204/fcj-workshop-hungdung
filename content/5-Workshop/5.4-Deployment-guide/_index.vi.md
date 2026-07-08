---
title: "Hướng dẫn triển khai"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Phần này trình bày quy trình triển khai HaShop end-to-end. Mỗi bước được tách thành một trang riêng để dễ theo dõi và kiểm tra kết quả.

**5.4.1:** [Tạo S3 bucket chứa Lambda artifact](5.4.1-Create-lambda-artifact-bucket/)

**5.4.2:** [Package và upload Lambda PostConfirmation](5.4.2-Package-upload-lambda/)

**5.4.3:** [Tạo ECR repository và push Docker image](5.4.3-Push-docker-images/)

**5.4.4:** [Clone CloudFormation repository](5.4.4-Clone-cloudformation/)

**5.4.5:** [Triển khai CloudFormation stack](5.4.5-Deploy-cloudformation-stacks/)

**5.4.6:** [Chuẩn bị và publish frontend](5.4.6-Prepare-frontend/)

> **Lưu ý:** Các đường dẫn trực tiếp đến thư mục trong hướng dẫn, ví dụ `mkdir D:\HaShop-release`, `cd D:\HaShop-release` hoặc `cd D:\HaShop-release\cognito-post-confirmation-db`, chỉ mang giá trị tham khảo. Vui lòng thay thế bằng đường dẫn thực tế đến thư mục workspace trên máy của bạn.
