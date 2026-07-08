---
title: "Bản đề xuất"
date: 2026-07-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# HaShop E-commerce Platform
## Giải pháp thương mại điện tử Microservices trên AWS kết hợp Event-Driven Architecture

### 1. Tóm tắt điều hành

HaShop là nền tảng thương mại điện tử bán quần áo trực tuyến được xây dựng trên Amazon Web Services (AWS). Dự án áp dụng kiến trúc **Microservices** kết hợp **Event-Driven Architecture** nhằm tạo ra một hệ thống có khả năng mở rộng, dễ bảo trì, bảo mật tốt và phù hợp để triển khai trong môi trường cloud-native.

Hệ thống sử dụng **Amazon ECS Fargate** để chạy các backend service dạng container, **Amazon Cognito** để xác thực người dùng, **Amazon RDS MySQL** để lưu trữ dữ liệu nghiệp vụ, **Amazon S3** và **Amazon CloudFront** để phân phối frontend và hình ảnh sản phẩm. Các tác vụ bất đồng bộ như xử lý thanh toán, cập nhật trạng thái đơn hàng và gửi email thông báo được triển khai bằng **Amazon SNS**, **Amazon SQS** và các worker service chạy trên ECS Fargate.

Mục tiêu của dự án không chỉ là xây dựng một website thương mại điện tử đầy đủ chức năng cơ bản, mà còn là cơ hội để thực hành các thành phần AWS quan trọng như networking, security, container orchestration, service discovery, event-driven processing, observability và infrastructure as code thông qua AWS CloudFormation.

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Các website thương mại điện tử truyền thống nếu xây dựng theo mô hình monolithic thường gặp khó khăn khi hệ thống phát triển lớn hơn. Khi toàn bộ chức năng như người dùng, sản phẩm, giỏ hàng, đơn hàng, thanh toán và thông báo nằm trong cùng một ứng dụng, việc sửa đổi hoặc mở rộng một chức năng có thể ảnh hưởng đến toàn bộ hệ thống.

Bên cạnh đó, một số tác vụ như thanh toán, gửi email hoặc cập nhật trạng thái đơn hàng không nhất thiết phải xử lý đồng bộ trong cùng request của người dùng. Nếu mọi tác vụ đều xử lý trực tiếp trong luồng chính, hệ thống dễ bị chậm, khó mở rộng và khó kiểm soát lỗi khi một thành phần phụ gặp sự cố.

#### Giải pháp đề xuất

HaShop tách hệ thống thành nhiều microservice độc lập như User Service, Product Service, Inventory Service, Cart Service, Order Service, Payment Service và các worker xử lý bất đồng bộ. Các service được đóng gói bằng Docker và triển khai trên Amazon ECS Fargate trong private subnet để giảm nhu cầu quản lý máy chủ và tăng mức độ bảo mật.

Frontend được triển khai dưới dạng static website trên Amazon S3 và phân phối qua Amazon CloudFront. CloudFront Function được dùng để rewrite các clean route như `/login`, `/profile`, `/cart` hoặc `/products/:id` về các file HTML tương ứng. Các API request có tiền tố `/api/*` được CloudFront chuyển tiếp đến Application Load Balancer, sau đó ALB định tuyến request đến backend service phù hợp.

Amazon Cognito quản lý đăng ký, xác nhận tài khoản, đăng nhập và phát hành JWT token. Backend service kiểm tra token bằng middleware trước khi xử lý các API cần xác thực. Sau khi người dùng xác nhận tài khoản, Cognito kích hoạt Lambda PostConfirmation để đồng bộ thông tin user vào Amazon RDS MySQL.

Các tác vụ bất đồng bộ được xử lý bằng Amazon SNS và Amazon SQS. Khi người dùng đặt hàng bằng MOMO hoặc BANK, Order Service publish sự kiện thanh toán vào SNS Topic. Message được fanout đến SQS Queue để Payment Worker xử lý. Sau đó kết quả thanh toán tiếp tục được publish qua SNS/SQS để Order Worker cập nhật trạng thái đơn hàng và Notification Worker gửi email thông báo.

#### Lợi ích

Giải pháp giúp hệ thống dễ mở rộng theo từng domain nghiệp vụ, giảm rủi ro khi triển khai thay đổi và giúp từng service có thể được kiểm thử độc lập. Event-Driven Architecture giúp các tác vụ nặng hoặc không cần phản hồi ngay được tách khỏi request chính, từ đó cải thiện trải nghiệm người dùng và tăng khả năng chịu lỗi của hệ thống.

### 3. Kiến trúc giải pháp

HaShop sử dụng kiến trúc cloud-native trên AWS với các lớp chính: **Edge/Frontend Layer**, **Authentication Layer**, **Application Layer**, **Async Processing Layer**, **Data Layer** và **Management/Observability Layer**.

![HaShop AWS Architecture](/fcj-workshop-haidang/images/2-Proposal/hashop-architecture.png)

Ở lớp Edge/Frontend, người dùng truy cập website thông qua Amazon CloudFront. CloudFront phân phối static frontend từ S3, áp dụng CloudFront Function cho route rewrite và chuyển tiếp request `/api/*` đến Application Load Balancer. AWS WAF được đặt ở phía trước để tăng khả năng bảo vệ ứng dụng khỏi các request bất thường.

Authentication Layer sử dụng Amazon Cognito để quản lý user pool, app client, xác nhận tài khoản và phát hành JWT token. Lambda PostConfirmation được dùng để ghi thông tin người dùng mới vào database sau khi tài khoản được xác nhận.

Application Layer được triển khai bằng Amazon ECS Fargate. Các API service và worker service chạy trong private subnet, không gán public IP. Các service giao tiếp nội bộ thông qua ECS Service Connect kết hợp AWS Cloud Map namespace.

Async Processing Layer sử dụng SNS và SQS. SNS Topic đóng vai trò phát sự kiện, SQS Queue lưu message để worker xử lý theo cơ chế long polling. Payment Worker, Order Worker và Notification Worker xử lý message độc lập, giúp giảm coupling giữa các service.

Data Layer sử dụng Amazon RDS MySQL triển khai trong private DB subnet và có Multi-AZ Replication để tăng tính sẵn sàng. Hệ thống chia dữ liệu theo domain như user, product, inventory, cart, order và payment để phù hợp với kiến trúc microservices.

Management/Observability Layer sử dụng AWS Secrets Manager để lưu thông tin nhạy cảm và Amazon CloudWatch Logs để thu thập log từ ECS service, worker và Lambda.

### 4. Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò trong hệ thống |
|---|---|
| Amazon CloudFront | Phân phối frontend, forward API request và phân phối hình ảnh sản phẩm. |
| CloudFront Function | Rewrite clean route của frontend về các file HTML tương ứng. |
| AWS WAF | Bảo vệ ứng dụng ở lớp edge trước các request bất thường. |
| Amazon S3 | Lưu trữ static frontend và hình ảnh sản phẩm. |
| Application Load Balancer | Nhận request `/api/*` và route đến ECS service theo path. |
| Amazon ECS Fargate | Chạy backend API service và worker service dạng container. |
| ECS Service Connect / AWS Cloud Map | Hỗ trợ service discovery và giao tiếp nội bộ giữa các microservice. |
| Amazon Cognito | Quản lý đăng ký, xác nhận tài khoản, đăng nhập và JWT token. |
| AWS Lambda | Xử lý PostConfirmation trigger từ Cognito. |
| Amazon RDS MySQL | Lưu trữ dữ liệu user, product, inventory, cart, order và payment. |
| Amazon SNS | Phát sự kiện thanh toán và thông báo. |
| Amazon SQS | Lưu message để worker xử lý bất đồng bộ. |
| Amazon SES | Gửi email thông báo cho người dùng và quản trị viên. |
| AWS Secrets Manager | Lưu trữ database credentials, Cognito config và internal API key. |
| Amazon CloudWatch Logs | Theo dõi log và hỗ trợ kiểm tra lỗi. |
| AWS IAM | Phân quyền cho ECS task, Lambda và các AWS service. |
| AWS CloudFormation | Tự động hóa triển khai hạ tầng. |
| VPC Endpoints | Cho phép ECS trong private subnet truy cập AWS service qua đường private. |

### 5. Thiết kế thành phần

- **Frontend:** Giao diện HTML/CSS/JavaScript được lưu trên S3 và phân phối qua CloudFront.
- **User Service:** Xử lý đăng ký, đăng nhập, hồ sơ người dùng và quản trị user; tích hợp với Amazon Cognito.
- **Product Service:** Quản lý sản phẩm, danh mục, size, màu sắc, biến thể sản phẩm và hình ảnh.
- **Inventory Service:** Quản lý tồn kho theo từng biến thể sản phẩm.
- **Cart Service:** Quản lý giỏ hàng và kiểm tra thông tin sản phẩm/tồn kho thông qua service nội bộ.
- **Order Service:** Xử lý đặt hàng, quản lý trạng thái đơn hàng và phát sự kiện thanh toán.
- **Payment Service:** Quản lý phương thức thanh toán của người dùng.
- **Payment Worker:** Xử lý yêu cầu thanh toán bất đồng bộ từ SQS.
- **Order Worker:** Nhận kết quả thanh toán và cập nhật trạng thái đơn hàng.
- **Notification Worker:** Nhận sự kiện thông báo và gửi email qua Amazon SES.
- **RDS MySQL:** Lưu dữ liệu chính của hệ thống như user, product, inventory, cart, order và payment.

### 6. Triển khai kỹ thuật

#### Các giai đoạn triển khai

1. **Phân tích yêu cầu và thiết kế kiến trúc:** Xác định các chức năng chính của website bán quần áo, lựa chọn kiến trúc microservices, thiết kế sơ đồ AWS, xác định backend service, database và luồng xử lý bất đồng bộ.
2. **Xây dựng backend microservices:** Phát triển các service Node.js/Express gồm User, Product, Inventory, Cart, Order, Payment và Notification. Mỗi service có trách nhiệm riêng, sử dụng database riêng theo domain và giao tiếp qua API nội bộ.
3. **Tích hợp xác thực và phân quyền:** Tích hợp Amazon Cognito để quản lý đăng ký, xác nhận tài khoản, đăng nhập và JWT token. Backend service dùng middleware để verify access token. Cognito Groups được dùng để phân quyền Admin và Customer.
4. **Xây dựng luồng event-driven:** Sử dụng SNS và SQS để xử lý thanh toán và notification bất đồng bộ. Các worker service chạy trên ECS Fargate và long-poll SQS Queue để xử lý message.
5. **Triển khai hạ tầng AWS và kiểm thử:** Ban đầu hệ thống được triển khai thủ công trên AWS Console để kiểm tra từng thành phần như VPC, RDS, ECS, ALB, CloudFront, SNS/SQS, Cognito và Lambda.
6. **Kiểm thử, sửa lỗi và tối ưu:** Kiểm thử toàn bộ flow từ frontend đến backend, bao gồm đăng ký, đăng nhập, quản lý sản phẩm, thêm vào giỏ hàng, checkout COD, checkout MOMO/BANK, xử lý worker và gửi email.

#### Yêu cầu kỹ thuật

- **Backend:** Node.js, Express.js, MySQL client, AWS SDK, Cognito JWT verifier.
- **Frontend:** HTML, CSS, JavaScript, static hosting trên S3.
- **Containerization:** Docker image cho từng service, lưu trên Amazon ECR.
- **Compute:** Amazon ECS Fargate cho API service và worker service.
- **Database:** Amazon RDS MySQL.
- **Authentication:** Amazon Cognito User Pool, App Client và Cognito Groups.
- **Networking:** VPC, public subnet, private app subnet, private DB subnet, NAT Gateway, VPC Endpoint và Security Group.
- **Event-driven processing:** Amazon SNS, Amazon SQS và ECS worker service.
- **Storage:** S3 Frontend Bucket và S3 Product Images Bucket.
- **Observability:** Amazon CloudWatch Logs.

### 7. Lộ trình & mốc triển khai

| Giai đoạn | Nội dung chính |
|---|---|
| Giai đoạn 1: Phân tích và thiết kế | Xác định yêu cầu nghiệp vụ, thiết kế kiến trúc microservices trên AWS, xác định service, database và luồng xử lý chính. |
| Giai đoạn 2: Phát triển backend và frontend | Xây dựng các service User, Product, Inventory, Cart, Order, Payment, Notification và giao diện HTML/CSS/JavaScript. |
| Giai đoạn 3: Triển khai AWS | Tạo VPC, subnet, security group, RDS, Cognito, Lambda, S3, CloudFront, ECS, ALB, SNS và SQS bằng AWS Console. |
| Giai đoạn 4: Kiểm thử và tối ưu | Kiểm thử chức năng, sửa lỗi cấu hình, bổ sung VPC Endpoint, hoàn thiện sơ đồ kiến trúc và chuẩn bị CloudFormation template. |

### 8. Ước tính ngân sách

Chi phí của hệ thống phụ thuộc vào thời gian chạy ECS Fargate, số lượng RDS instance, NAT Gateway, CloudFront, S3, SNS/SQS và lượng request thực tế. Do dự án phục vụ mục đích học tập và demo, cấu hình được giữ ở mức nhỏ nhất có thể.

| Nhóm chi phí | Cấu hình ước tính | Chi phí/tháng |
|---|---|---:|
| Amazon ECS Fargate | 9 tasks, mỗi task 0.5 vCPU và 1 GB RAM | 198.43 USD |
| Amazon RDS MySQL | 2 DB instances db.t3.micro, Single-AZ, 20 GB storage mỗi DB | 43.48 USD |
| Application Load Balancer | 1 ALB chạy liên tục, ước tính 1 LCU | 25.55 USD |
| NAT Gateway | 1 NAT Gateway chạy liên tục, khoảng 1 GB data xử lý | 43.13 USD |
| VPC Endpoints | 6 Interface Endpoints trên 2 AZ; S3 Gateway Endpoint không tính phí hourly | 122.65 USD |
| Amazon S3 | Khoảng 6 GB lưu frontend, hình ảnh sản phẩm và artifact | 0.16 USD |
| Amazon CloudFront | Khoảng 1 GB data transfer và 20,000 HTTPS requests | 0.00 USD |
| AWS WAF | 1 CloudFront Web ACL, 3 AWS Managed Rule Groups, 1 rate-based rule, khoảng 20,000 requests/tháng | 9.01 USD |
| Amazon SNS/SQS | Khoảng 10,000 messages/tháng | 0.01 USD |
| Amazon Cognito | Khoảng 20 monthly active users | 0.00 USD |
| AWS Lambda | Khoảng 1,000 invocations/tháng, 256 MB | 0.00 USD |
| CloudWatch Logs | Khoảng 1 GB log ingested và 1 GB log stored | 0.53 USD |
| Amazon SES | Khoảng 1,000 email/tháng | 0.10 USD |
| AWS Secrets Manager | 4 secrets, khoảng 10,000 API calls/tháng | 1.65 USD |
| Amazon ECR | 7 Docker images, khoảng 3.5 GB private repository storage | 0.35 USD |

**Tổng chi phí ước tính:** khoảng **444.92 USD/tháng**, tương đương **5,339.04 USD cho 12 tháng**.

### 9. Đánh giá rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất | Chiến lược giảm thiểu |
|---|---|---|---|
| Sai cấu hình networking hoặc security group | Cao | Trung bình | Chuẩn hóa hạ tầng bằng CloudFormation, kiểm tra target group health check và rule security group sau mỗi lần deploy. |
| Sai cấu hình Cognito hoặc JWT verification | Cao | Trung bình | Kiểm thử đăng ký, đăng nhập, token expiration và API authorization trước khi tích hợp toàn hệ thống. |
| Sai cấu hình Service Connect | Trung bình đến cao | Trung bình | Kiểm tra DNS nội bộ, namespace Cloud Map và log của từng service. |
| Worker không xử lý được message từ SQS | Cao | Trung bình | Theo dõi SQS, DLQ và CloudWatch Logs của worker service. |
| Vượt chi phí AWS | Trung bình | Trung bình | Dùng AWS Budget, tắt tài nguyên không cần thiết và giữ cấu hình ở mức demo. |

Nếu một service gặp lỗi, nhóm có thể kiểm tra log riêng trong CloudWatch và cập nhật task definition hoặc CloudFormation template. Nếu worker xử lý message lỗi, có thể kiểm tra SQS Queue, DLQ và log worker để xác định nguyên nhân trước khi replay hoặc xử lý lại message.

### 10. Kết quả kỳ vọng

Sau khi hoàn thành, HaShop cung cấp một hệ thống thương mại điện tử đầy đủ các chức năng cơ bản gồm đăng ký, đăng nhập, quản lý sản phẩm, quản lý tồn kho, giỏ hàng, đặt hàng, thanh toán mô phỏng và gửi email thông báo.

Về mặt kỹ thuật, dự án chứng minh khả năng áp dụng kiến trúc Microservices và Event-Driven Architecture trên AWS trong một bài toán thực tế. Nhóm có thể nắm được cách triển khai ứng dụng container trên ECS Fargate, sử dụng Cognito để xác thực, thiết kế database theo domain, xử lý tác vụ bất đồng bộ bằng SNS/SQS và phân phối frontend bằng S3/CloudFront.

Về giá trị dài hạn, dự án có thể tiếp tục mở rộng thành một nền tảng thương mại điện tử hoàn chỉnh hơn, bổ sung các chức năng như đặt may theo số đo, quản lý khuyến mãi, đánh giá sản phẩm, thanh toán thật với cổng thanh toán bên ngoài, dashboard phân tích doanh thu và CI/CD tự động.
