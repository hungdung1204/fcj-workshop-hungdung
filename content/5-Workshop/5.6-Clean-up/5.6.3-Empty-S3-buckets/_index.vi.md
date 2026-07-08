---
title: "Empty S3 bucket do foundation tạo"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

Trước khi xóa foundation stack, cần empty các S3 bucket do stack tạo. CloudFormation không thể tự xóa bucket vẫn còn object bên trong.

Kết quả mong đợi:

- Tài nguyên cần xóa được xóa thành công.
- Không còn stack phụ thuộc bị kẹt ở trạng thái xóa lỗi.
- AWS account không còn giữ tài nguyên lab có thể phát sinh chi phí.

![Empty S3 bucket do foundation tạo](/fcj-workshop-haidang/images/5-Workshop/hashop-test-cleanup/image13.png)
