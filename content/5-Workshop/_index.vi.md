---
title: "Workshop"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

Chương này trình bày workshop triển khai nền tảng thương mại điện tử HaShop trên AWS. Nội dung được sắp xếp theo luồng: tổng quan dự án, mô tả kiến trúc, điều kiện chuẩn bị, triển khai, kiểm thử và dọn dẹp tài nguyên.

**5.1:** [Tổng quan](5.1-Overview/)

Giới thiệu bối cảnh dự án, bài toán cần giải quyết, người dùng mục tiêu, kết quả mong muốn và tiêu chí đánh giá thành công.

**5.2:** [Mô tả kiến trúc](5.2-Architecture/)

Trình bày sơ đồ kiến trúc, các lớp hệ thống, luồng request chính, dịch vụ AWS sử dụng, lý do lựa chọn dịch vụ, bảo mật/IAM, logging/monitoring, alert, tối ưu chi phí và clean-up.

**5.3:** [Điều kiện chuẩn bị](5.3-Prerequisite/)

Liệt kê quyền tài khoản AWS, region và công cụ cục bộ cần chuẩn bị trước khi triển khai.

**5.4:** [Hướng dẫn triển khai](5.4-Deployment-guide/)

Cung cấp các bước triển khai end-to-end cho Lambda artifact, container image, CloudFormation stack và bản phát hành frontend.

**5.5:** [Kiểm thử và xác thực](5.5-Test-validation/)

Kiểm tra frontend, đăng nhập, ECS service, quản lý sản phẩm, luồng đặt hàng, CloudWatch log và metric của WAF.

**5.6:** [Dọn dẹp tài nguyên](5.6-Clean-up/)

Xóa các tài nguyên lab theo đúng thứ tự để tránh phát sinh chi phí AWS không cần thiết.
