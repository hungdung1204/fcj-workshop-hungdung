---
title: "Xem log trong CloudWatch"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.5.6. </b> "
---

Trên AWS Console, mở:

```text
CloudWatch -> Log groups
```

Tìm log group của các ECS service và Lambda liên quan. Kiểm tra log của:

- `user-api`
- `product-api`
- `inventory-api`
- `cart-api`
- `order-api`
- `payment-api`
- `payment-worker`
- `order-worker`
- `notification-worker`
- Lambda PostConfirmation

![CloudWatch log groups](/images/5-Workshop/hashop-test-cleanup/image9.png)

Kết quả mong đợi:

- Không có lỗi nghiêm trọng như `CannotPullContainerError`.
- Không có lỗi kết nối database.
- Không có lỗi JWT verify failed.
- Không có lỗi SQS permission denied.
