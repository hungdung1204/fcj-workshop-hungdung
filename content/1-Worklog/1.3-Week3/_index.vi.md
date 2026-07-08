---
title: "Worklog Tuần 3"
date: 2026-05-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---



### Mục tiêu tuần 3:
* Tìm hiểu Amazon VPC và các thành phần networking cơ bản trên AWS.
* Nắm được VPC, Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group và Network ACL.
* Thực hành thiết kế mô hình mạng có public subnet và private subnet.
* Làm quen với VPC Flow Logs, Reachability Analyzer và Site-to-Site VPN ở mức tổng quan.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 | - Tìm hiểu Amazon VPC <br> - Nắm khái niệm CIDR block <br> - Phân biệt Default VPC và Custom VPC <br> - Ghi chú vai trò của VPC trong kiến trúc AWS | 01/05/2026 | 01/05/2026 | <https://000003.awsstudygroup.com/> <br><https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i> |
| 2 | - Tìm hiểu Subnet <br> - Phân biệt Public Subnet và Private Subnet <br> - Tìm hiểu Route Table <br> - Phác thảo mô hình VPC có public/private subnet | 04/05/2026 | 04/05/2026 | <https://000003.awsstudygroup.com/> |
| 3 | - Tìm hiểu Internet Gateway và NAT Gateway <br> - Ghi chú cách tài nguyên trong public subnet truy cập Internet <br> - Ghi chú cách tài nguyên trong private subnet truy cập Internet thông qua NAT Gateway <br> - Lưu ý chi phí khi sử dụng NAT Gateway | 05/05/2026 | 05/05/2026 | <https://000003.awsstudygroup.com/> |
| 4 | - Tìm hiểu Security Group và Network ACL <br> - Phân biệt stateful và stateless <br> - Thực hành thiết kế rule inbound/outbound cơ bản <br> - Ghi chú nguyên tắc chỉ mở port cần thiết | 06/05/2026 | 06/05/2026 | <https://000003.awsstudygroup.com/> |
| 5 | - Tìm hiểu VPC Flow Logs, Reachability Analyzer và Session Manager ở mức tổng quan <br> - Làm quen với mô hình Site-to-Site VPN <br> - Hoàn thiện sơ đồ network layer gồm VPC, Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group và NACL | 07/05/2026 | 07/05/2026 | <https://000003.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:
**Tổng quát:**

Trong tuần này tôi đã hiểu được cách AWS xây dựng network layer thông qua Amazon VPC. Tôi biết cách phân biệt public subnet, private subnet và cách kiểm soát truy cập mạng bằng Security Group và Network ACL.

**Lý thuyết đã học:**

* Amazon VPC và CIDR block.
* Public Subnet và Private Subnet.
* Route Table, Internet Gateway và NAT Gateway.
* Security Group và Network ACL.
* VPC Flow Logs, Reachability Analyzer và Site-to-Site VPN.

**Thực hành với bài lab:**

* Phác thảo mô hình VPC.
* Thiết kế public subnet và private subnet.
* Cấu hình route table ở mức cơ bản.
* Xác định luồng truy cập Internet thông qua Internet Gateway và NAT Gateway.
* Hoàn thiện sơ đồ network layer trên AWS.
