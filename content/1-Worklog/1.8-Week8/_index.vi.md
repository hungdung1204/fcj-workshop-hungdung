---
title: "Worklog Tuần 8"
date: 2026-06-05
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Mục tiêu tuần 8:
* Tìm hiểu Elastic Load Balancing và Auto Scaling Group.
* Nắm được cách triển khai ứng dụng có khả năng mở rộng.
* Hiểu vai trò của Launch Template, Target Group và Health Check.
* Thực hành triển khai ứng dụng với Auto Scaling Group.
* Hiểu mô hình high availability trên nhiều Availability Zone.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 | - Tìm hiểu tổng quan Elastic Load Balancing <br> - Nắm vai trò của Application Load Balancer <br> - Ghi chú cách ALB phân phối request đến nhiều EC2 Instance <br> - Xác định vị trí ALB trong sơ đồ kiến trúc | 05/06/2026 | 05/06/2026 | <https://000006.awsstudygroup.com/> <br><https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i> |
| 2 | - Tìm hiểu Target Group và Health Check <br> - Cấu hình hoặc mô phỏng Target Group cho EC2 <br> - Kiểm tra trạng thái healthy/unhealthy <br> - Ghi chú cách health check giúp phát hiện instance lỗi | 08/06/2026 | 08/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Tìm hiểu Launch Template <br> - Nắm thông tin AMI, instance type, key pair, security group trong launch template <br> - Ghi chú vai trò của launch template khi tạo Auto Scaling Group | 09/06/2026 | 09/06/2026 | <https://000006.awsstudygroup.com/> |
| 4 | - Tìm hiểu Auto Scaling Group <br> - Nắm desired capacity, minimum capacity và maximum capacity <br> - Ghi chú cách ASG tự động thêm hoặc giảm EC2 Instance <br> - Thực hành hoặc mô phỏng cấu hình Auto Scaling | 10/06/2026 | 10/06/2026 | <https://000006.awsstudygroup.com/> |
| 5 | - Hoàn thiện sơ đồ kiến trúc có ALB và Auto Scaling Group <br> - Thể hiện luồng User → ALB → Target Group → EC2 Auto Scaling Group <br> - Ghi chú lợi ích và chi phí khi triển khai Multi-AZ <br> - Dọn dẹp tài nguyên sau khi hoàn thành lab | 11/06/2026 | 11/06/2026 | <https://000006.awsstudygroup.com/> |


### Kết quả đạt được tuần 8:
**Tổng quát:**

Trong tuần này tôi đã hiểu cách triển khai hệ thống có khả năng mở rộng và tính sẵn sàng cao bằng Application Load Balancer và Auto Scaling Group. Tôi biết cách mô tả luồng request từ người dùng đến nhiều EC2 Instance thông qua ALB.

**Lý thuyết đã học:**

* Elastic Load Balancing.
* Application Load Balancer.
* Target Group và Health Check.
* Launch Template.
* Auto Scaling Group.
* Multi-AZ và high availability.

**Thực hành với bài lab:**

* Tạo hoặc mô phỏng Target Group.
* Cấu hình Health Check.
* Tìm hiểu Launch Template.
* Cấu hình Auto Scaling Group.
* Vẽ sơ đồ User → ALB → Target Group → EC2 Auto Scaling Group.
* Dọn dẹp tài nguyên sau khi hoàn thành lab.
