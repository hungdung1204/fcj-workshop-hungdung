---
title: "Kiểm tra luồng đặt hàng"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

Thực hiện các kịch bản đặt hàng trên website:

1. Admin tạo category, size, color, product và tồn kho.
2. Customer thêm sản phẩm vào giỏ hàng.
3. Customer checkout bằng COD.
4. Customer checkout bằng phương thức MOMO/BANK demo.
5. Kiểm tra trạng thái đơn hàng.

Kết quả mong đợi:

- Đơn COD được xác nhận trực tiếp.
- Đơn MOMO/BANK chuyển sang trạng thái chờ thanh toán.
- `payment-worker` xử lý message từ SQS.
- `order-worker` cập nhật trạng thái đơn hàng sau khi có kết quả thanh toán.

![Luồng checkout đơn hàng](/images/5-Workshop/hashop-test-cleanup/image7.png)

![Kiểm tra trạng thái đơn hàng](/images/5-Workshop/hashop-test-cleanup/image8.png)
