---
title: "Triển khai CloudFormation stack"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

## Stack 0: Global WAF

Chuyển AWS Console sang region **United States (N. Virginia) `us-east-1`**. Mở CloudFormation và tạo stack bằng template có sẵn. Upload file `00-cf-hashop-global-waf.yaml`.

![Upload WAF template](/images/5-Workshop/hashop-deployment/image7.png)

Chọn **Next**.

![Thông tin WAF stack](/images/5-Workshop/hashop-deployment/image8.png)

Đặt stack name là `HaShop-WAF`, sau đó tiếp tục chọn **Next**.

![Đặt tên WAF stack](/images/5-Workshop/hashop-deployment/image9.png)

Kiểm tra lại cấu hình và chọn **Submit**.

![Submit WAF stack](/images/5-Workshop/hashop-deployment/image10.png)

Sau khi stack tạo xong, mở tab **Outputs** và ghi lại `FrontendWebAclArn`. ARN này sẽ được dùng khi triển khai foundation stack.

## Stack 01: Foundation

Chuyển AWS Console về region **Singapore `ap-southeast-1`**. Tạo CloudFormation stack mới và upload file `01-cf-hashop-foundation.yaml`.

![Upload foundation template](/images/5-Workshop/hashop-deployment/image11.png)

Đặt stack name chính xác là `cf-hashop-foundation`.

![Đặt tên foundation stack](/images/5-Workshop/hashop-deployment/image12.png)

Nhập các parameter cần thiết:

- `DbPassword`: mật khẩu cho database.
- `InternalApiKey`: internal API key gồm 16 ký tự.
- `AdminEmail`: email của người quản trị.
- `SesFromEmail`: email gửi đã được xác thực trong SES.

![Parameter của foundation stack](/images/5-Workshop/hashop-deployment/image13.png)

Thiết lập parameter cho Lambda artifact:

- `LambdaCodeS3Bucket`: `hashop-artifacts-<ACCOUNT_ID>-ap-southeast-1`
- `LambdaCodeS3Key`: `lambda/post-confirmation/function.zip`
- `FrontendWebAclArn`: ARN đã lấy từ Stack 0

![Parameter Lambda và WAF](/images/5-Workshop/hashop-deployment/image14.png)

Tiếp tục đến trang review, tích chọn xác nhận CloudFormation có thể tạo IAM resource với custom name, rồi submit stack.

Nếu ECS task bị fail do IAM role chưa kịp propagate, có thể delete stack bị lỗi và chạy lại.

## Stack 02: ECS services

Đợi Stack 01 triển khai xong. Quá trình này có thể mất khoảng 20 phút. Sau đó tạo CloudFormation stack mới bằng file `02-cf-hashop-ecs-services.yaml`. Đặt stack name là `cf-hashop-ecs`.

![Tạo ECS services stack](/images/5-Workshop/hashop-deployment/image15.png)

Ở phần container image, thay `<ACCOUNT_ID>` bằng AWS account ID thật.

![Parameter container image của ECS](/images/5-Workshop/hashop-deployment/image16.png)

## Stack 03: Database initialization

Stack 03 có thể bắt đầu sau khi Stack 02 đã được submit. Tạo CloudFormation stack mới bằng file `03-cf-hashop-db-init.yaml`.

![Upload database initialization template](/images/5-Workshop/hashop-deployment/image17.png)

Đặt tên cho database initialization stack.

![Đặt tên database initialization stack](/images/5-Workshop/hashop-deployment/image18.png)

Kiểm tra lại cấu hình stack.

![Review database initialization stack](/images/5-Workshop/hashop-deployment/image19.png)

Tích chọn xác nhận CloudFormation có thể tạo IAM resource với custom name, sau đó submit stack.
