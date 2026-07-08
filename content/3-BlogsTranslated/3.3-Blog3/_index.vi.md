---
title: "Blog 3"
date: 2026-07-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Giảm Lãng Phí EC2 - Right-Sizing Tự Động Với Compute Optimizer, CloudWatch Và Automation

## 1. Hóa đơn EC2: nỗi lo không của riêng ai

Khi vận hành hạ tầng AWS, chi phí EC2 thường là một trong những phần dễ tăng nhất. Nhiều đội triển khai EC2 theo hướng "chọn dư cho chắc", copy cấu hình từ dự án cũ hoặc không rà soát lại sau khi workload thay đổi. Kết quả là nhiều instance chạy quá lớn so với nhu cầu thực tế.

![EC2 Right-Sizing Automation](/images/3-BlogsTranslated/blog3/image1.png)

## 2. Vấn đề: vì sao lãng phí xảy ra?

Một số nguyên nhân phổ biến:

* Overprovisioning: chọn instance lớn hơn nhu cầu thật.
* Thiếu đánh giá định kỳ: không có quy trình rà soát và điều chỉnh.
* Quy trình thủ công: kỹ sư phải tự phân tích metrics và tự quyết định thay đổi.

Hậu quả là hóa đơn tăng không cần thiết, tài nguyên bị sử dụng kém hiệu quả và đội ngũ kỹ thuật mất thời gian cho các thao tác tối ưu lặp lại.

## 3. Giải pháp tổng quan: Right-sizing tự động

Mục tiêu của right-sizing tự động là phát hiện instance đang overprovisioned hoặc underutilized, sau đó đề xuất hoặc áp dụng thay đổi một cách an toàn.

Bộ giải pháp gồm:

* **AWS Compute Optimizer:** phân tích CPU, memory, network và disk I/O để đưa ra recommendation.
* **CloudWatch:** thu thập metrics lịch sử, tạo dashboard và cảnh báo.
* **Automation:** sử dụng Lambda, SSM Automation, Systems Manager Run Command hoặc Step Functions để áp dụng thay đổi.
* **Governance:** yêu cầu phê duyệt trước khi thay đổi production, sử dụng tagging và audit trail.

## 4. Flow hoạt động đề xuất

1. Compute Optimizer thu thập dữ liệu trong 14-30 ngày và trả về recommendation theo từng instance.
2. Hệ thống lọc các recommendation có độ tin cậy cao, bỏ qua instance stateful, database hoặc tài nguyên quan trọng dựa trên tag.
3. Thử nghiệm cấu hình mới trên staging bằng AMI hoặc Launch Template.
4. Áp dụng cho production bằng rolling update trong Auto Scaling Group hoặc quy trình stop/modify/start với maintenance window.
5. CloudWatch theo dõi latency, error rate, CPU, memory và kích hoạt rollback nếu vượt ngưỡng.

## 5. Hướng dẫn triển khai nhanh

* Bật Compute Optimizer cho account hoặc organization.
* Đợi ít nhất 14 ngày để có dữ liệu đủ tốt.
* Bật export recommendation sang S3 hoặc lấy dữ liệu qua API/SDK.
* Viết Lambda hoặc script lọc recommendation hằng ngày.
* Tạo workflow phê duyệt qua Slack hoặc pull request.
* Áp dụng thay đổi bằng SSM Automation hoặc rolling update.
* Thiết lập CloudWatch dashboard và alarm cho rollback.

## 6. Checklist triển khai

* Bật Compute Optimizer.
* Thiết lập export recommendation.
* Gắn tag `do-not-rightsize` cho instance không được tự động thay đổi.
* Lọc recommendation theo confidence, môi trường và tag quan trọng.
* Kiểm thử trên staging trước khi áp dụng production.
* Lưu audit trail bằng CloudTrail và S3 logs.

## 7. Khi nào nên và không nên áp dụng?

Phù hợp với stateless web server, API server, worker node, batch worker, Auto Scaling Group-managed fleet và workload có thể khởi động lại nhanh.

Cần cân nhắc kỹ với database production chạy trên EC2, workload có local state, instance có licensing gắn với phần cứng hoặc ứng dụng có boot sequence đặc thù.

## 8. Bài học quan trọng

* Bắt đầu từ non-production trước.
* Chuẩn hóa tagging và inventory.
* Luôn có quy trình test và rollback.
* Kết hợp automation với phê duyệt cho workload quan trọng.
* Kết hợp right-sizing với Savings Plans và Reserved Instances để tối ưu chi phí dài hạn.

## 9. Ví dụ policy lọc

Chỉ xem xét recommendation khi confidence từ 70% trở lên, instance không phải `prod-db`, uptime đủ dài, CPU median dưới 30% và memory median dưới 40% trong 14 ngày.

Với môi trường non-production có thể tự động phê duyệt. Với production, hệ thống nên tạo pull request hoặc Slack notification và chờ phê duyệt thủ công.

## 10. Kết luận

Right-sizing tự động là cách hiệu quả để giảm chi phí EC2 mà vẫn duy trì ổn định hệ thống. Khi kết hợp Compute Optimizer, CloudWatch và automation an toàn, doanh nghiệp có thể giảm lãng phí, giữ hiệu năng ổn định và giảm công việc thủ công cho kỹ sư vận hành.
