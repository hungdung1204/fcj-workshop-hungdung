---
title: "Check ECS services"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

In the AWS Console, open:

```text
Amazon ECS -> Clusters -> cf-hashop-test-cluster -> Services
```

Expected result:

- `user-api` is running.
- `product-api` is running.
- `inventory-api` is running.
- `cart-api` is running.
- `order-api` is running.
- `payment-api` is running.
- `payment-worker` is running.
- `order-worker` is running.
- `notification-worker` is running.

![ECS services running](/images/5-Workshop/hashop-test-cleanup/image2.png)
