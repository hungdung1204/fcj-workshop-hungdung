---
title: "Đăng nhập admin và kiểm tra authorization"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

Sử dụng tài khoản admin mặc định để đăng nhập:

```text
Email: admin@example.com
Password: Letmein123!!!
```

Sau khi đăng nhập, mở các chức năng dành cho admin như quản lý sản phẩm.

Kết quả mong đợi:

- Tài khoản admin đăng nhập thành công.
- Các trang chỉ dành cho admin có thể truy cập sau khi xác thực.
- Request cần authorization được gửi kèm token hợp lệ.
- Người dùng không bị chuyển ngược về trang đăng nhập ngoài ý muốn.
