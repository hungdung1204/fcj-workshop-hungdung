---
title: "Kiểm tra ECS services"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

Trên AWS Console, mở:

```text
Amazon ECS -> Clusters -> cf-hashop-test-cluster -> Services
```

Kết quả mong đợi:

- `user-api` đang chạy.
- `product-api` đang chạy.
- `inventory-api` đang chạy.
- `cart-api` đang chạy.
- `order-api` đang chạy.
- `payment-api` đang chạy.
- `payment-worker` đang chạy.
- `order-worker` đang chạy.
- `notification-worker` đang chạy.

![Các ECS service đang chạy](/images/5-Workshop/hashop-test-cleanup/image2.png)
