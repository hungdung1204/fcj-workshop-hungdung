---
title: "Worklog Tuần 6"
date: 2026-05-22
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---



### Mục tiêu tuần 6:
* Tìm hiểu Amazon RDS và các khái niệm database trên AWS.
* Phân biệt database tự quản lý trên EC2 và managed database bằng RDS.
* Tạo RDS Database Instance, cấu hình DB Subnet Group và Security Group.
* Kết nối EC2 với RDS và kiểm tra hoạt động của database.
* Tìm hiểu backup, restore và Multi-AZ ở mức cơ bản.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 | - Tìm hiểu tổng quan Amazon RDS <br> - Phân biệt relational database và non-relational database <br> - Ghi chú lợi ích của managed database trên AWS <br> - Tìm hiểu các database engine phổ biến trong RDS | 22/05/2026 | 22/05/2026 | <https://000005.awsstudygroup.com/> <br><https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i> |
| 2 | - Tìm hiểu DB Instance, DB Subnet Group và Security Group cho RDS <br> - Thiết kế database đặt trong private subnet <br> - Xác định cách EC2 trong public/private subnet kết nối đến RDS | 25/05/2026 | 25/05/2026 | <https://000005.awsstudygroup.com/> |
| 3 | - Thực hành tạo RDS Database Instance <br> - Cấu hình engine, storage, username, password và networking <br> - Kiểm tra database endpoint sau khi tạo <br> - Ghi chú thời gian khởi tạo RDS | 26/05/2026 | 26/05/2026 | <https://000005.awsstudygroup.com/> |
| 4 | - Kết nối EC2 với RDS <br> - Cấu hình Security Group để EC2 truy cập database <br> - Kiểm tra kết nối bằng command line hoặc database client <br> - Ghi chú lỗi thường gặp khi kết nối thất bại | 27/05/2026 | 27/05/2026 | <https://000005.awsstudygroup.com/> |
| 5 | - Tìm hiểu backup và restore trong RDS <br> - Tìm hiểu Multi-AZ ở mức tổng quan <br> - Cập nhật sơ đồ kiến trúc có EC2, RDS, private subnet và Security Group <br> - Dọn dẹp tài nguyên sau khi hoàn thành lab | 28/05/2026 | 28/05/2026 | <https://000005.awsstudygroup.com/> |


### Kết quả đạt được tuần 6:
**Tổng quát:**

Trong tuần này tôi đã hiểu vai trò của Amazon RDS trong việc triển khai database có quản lý trên AWS. Tôi biết cách tạo RDS Database Instance, cấu hình kết nối và mô tả kiến trúc ứng dụng có EC2 kết nối đến RDS.

**Lý thuyết đã học:**

* Relational database và non-relational database.
* Amazon RDS và managed database.
* DB Instance, DB Engine, DB Subnet Group.
* RDS endpoint và Security Group.
* Backup, restore và Multi-AZ trong RDS.

**Thực hành với bài lab:**

* Tạo RDS Database Instance.
* Cấu hình database trong private subnet.
* Cấu hình Security Group cho phép EC2 truy cập database.
* Kết nối EC2 với RDS.
* Kiểm tra database endpoint.
* Dọn dẹp tài nguyên sau khi hoàn thành lab.
