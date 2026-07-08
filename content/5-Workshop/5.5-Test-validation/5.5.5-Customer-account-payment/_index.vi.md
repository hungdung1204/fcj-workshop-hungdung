---
title: "Đăng ký, đăng nhập, chỉnh sửa thông tin và thêm phương thức thanh toán"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

Phần kiểm thử này dùng để xác thực luồng tài khoản customer trên giao diện HaShop. Mục tiêu là đảm bảo người dùng mới có thể đăng ký tài khoản, xác thực email, đăng nhập, chỉnh sửa thông tin cá nhân và thêm phương thức thanh toán trước khi tiếp tục kiểm tra luồng đặt hàng.

## Đăng ký tài khoản customer

Truy cập trang đăng ký tài khoản của HaShop, sau đó nhập các thông tin cơ bản gồm họ tên, email, số điện thoại, mật khẩu và xác nhận mật khẩu.

![Trang đăng ký tài khoản customer](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image1.png)

Sau khi gửi form đăng ký, kiểm tra email để lấy mã xác thực tài khoản. Nếu không thấy email trong hộp thư đến, cần kiểm tra thêm thư mục spam hoặc mail rác.

![Email chứa mã xác thực](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image2.png)

Nhập mã OTP trên trang xác thực email để kích hoạt tài khoản customer.

![Trang xác thực email](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image3.png)

![Xác thực email thành công](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image4.png)

## Đăng nhập bằng tài khoản customer

Sau khi hoàn tất xác thực, quay lại trang đăng nhập và đăng nhập bằng tài khoản customer vừa tạo.

![Trang đăng nhập customer](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image5.png)

## Chỉnh sửa hồ sơ và thêm phương thức thanh toán

Từ khu vực tài khoản customer, vào `Profile`, sau đó chọn chức năng chỉnh sửa hồ sơ.

![Trang hồ sơ customer](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image6.png)

Cập nhật thông tin cá nhân, thêm địa chỉ giao hàng và liên kết MoMo làm phương thức thanh toán.

![Chỉnh sửa hồ sơ và thêm phương thức thanh toán](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image7.png)

Kết quả mong đợi:

- Tài khoản customer được đăng ký thành công.
- Email OTP được gửi về đúng địa chỉ và có thể dùng để xác thực tài khoản.
- Customer có thể đăng nhập sau khi xác thực thành công.
- Thông tin hồ sơ, địa chỉ và phương thức thanh toán MoMo được lưu thành công.
