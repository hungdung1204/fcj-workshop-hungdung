---
title: "Blog 4"
date: 2026-07-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Giới Hạn Truy Cập AWS Management Console Vào Mạng Tin Cậy Bằng Sign-in Resource-Based Policies Và RCPs

Khi làm việc với hệ thống AWS ở quy mô doanh nghiệp, một nhiệm vụ quan trọng của đội Security là xây dựng data perimeter. Chúng ta thường dùng Service Control Policies (SCPs) để kiểm soát hành động trong tài khoản, nhưng có một cánh cổng rất nhạy cảm thường bị bỏ sót: trang đăng nhập AWS Management Console.

Nếu thông tin đăng nhập của nhân viên bị lộ, kẻ xấu vẫn có thể truy cập trang login từ bất kỳ đâu trên Internet. AWS Security Blog đã giới thiệu một giải pháp mới giúp hạn chế truy cập Console ngay từ vòng ngoài: Sign-in Resource-Based Policies kết hợp với Resource Control Policies (RCPs).

## 1. Nỗi đau cũ: vì sao chặn IP đăng nhập Console lại khó?

Trước đây, để đảm bảo nhân viên chỉ đăng nhập từ mạng công ty hoặc VPN, đội kỹ thuật thường phải xử lý nhiều vấn đề:

* Phụ thuộc vào Identity Provider như Okta, Entra ID hoặc IAM Identity Center.
* SCP chủ yếu kiểm soát hành vi sau khi principal đã vào hệ thống.
* Việc chặn trực tiếp hành động `aws-signin:SignIn` bằng SCP có thể tạo ra xung đột chính sách khó quản trị.

Hậu quả là trang login AWS Console vẫn phơi ra Internet công cộng, tạo cơ hội cho brute-force hoặc khai thác credential bị rò rỉ.

## 2. Giải pháp mới từ AWS

AWS cho phép xem hành động đăng nhập Console như một resource có thể gắn chính sách bảo vệ riêng.

**Sign-in Resource-based Policies** cho phép áp chính sách trực tiếp lên sign-in endpoint. Ví dụ: chỉ cho phép đăng nhập vào tài khoản nếu request đến từ IP nguồn hoặc VPC cụ thể.

**Resource Control Policies (RCPs)** là loại policy trong AWS Organizations. Nếu SCP kiểm soát principal được làm gì, thì RCP kiểm soát resource được phép tiếp cận bởi ai và từ đâu.

Khi áp dụng RCP ở cấp AWS Organizations, doanh nghiệp có thể yêu cầu mọi hành vi đăng nhập Console vào tài khoản con phải xuất phát từ expected networks.

## 3. Luồng hoạt động

1. Người dùng truy cập AWS Management Console và nhập thông tin đăng nhập.
2. AWS Sign-in tiếp nhận request và trích xuất ngữ cảnh mạng như IP nguồn, VPC Endpoint hoặc Verified Access.
3. Resource Control Policy ở cấp Organization đánh giá request đăng nhập.
4. RCP kiểm tra mạng nguồn có nằm trong danh sách expected networks hay không.
5. Nếu mạng không hợp lệ, AWS trả về DENY ngay tại bước đăng nhập. Nếu hợp lệ, người dùng tiếp tục qua MFA và các lớp SCP/IAM Policy.

## 4. Checklist triển khai an toàn

* Xác định chính xác dải IP public của văn phòng, VPN, proxy và corporate VPC.
* Chừa đường lui cho break-glass accounts để tránh tự khóa khỏi hệ thống.
* Bật RCPs trong AWS Organizations từ Management Account.
* Thử nghiệm trước trên Sandbox OU thay vì áp dụng toàn bộ organization.
* Theo dõi CloudTrail ConsoleLogin để phát hiện và tinh chỉnh các trường hợp bị chặn nhầm.

## Góc nhìn cá nhân

Việc AWS ra mắt RCPs và áp dụng cho Sign-in là một bước tiến quan trọng trong mô hình Zero Trust và Data Perimeter. Trước đây, chúng ta thường tập trung bảo vệ dữ liệu trong S3 hoặc DynamoDB nhưng vẫn để cổng Console mở rộng trên Internet. Việc đưa điều kiện mạng vào lớp đăng nhập giúp đơn giản hóa bài toán bảo mật vòng ngoài.

## Kết luận

Kiểm soát truy cập AWS Management Console dựa trên expected networks thông qua Sign-in Resource-Based Policies và RCPs là một giải pháp bảo mật quan trọng cho doanh nghiệp dùng AWS. Nó giúp giảm rủi ro credential bị lộ, chặn truy cập từ mạng không tin cậy và tăng cường data perimeter ở cấp độ tổ chức.

Nguồn tham khảo: <https://aws.amazon.com/vi/blogs/security/restrict-aws-management-console-access-to-expected-networks-with-sign-in-resource-based-policies-and-rcps/>
