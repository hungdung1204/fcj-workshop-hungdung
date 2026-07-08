---
title: "Review CloudWatch logs"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.5.6. </b> "
---

In the AWS Console, open:

```text
CloudWatch -> Log groups
```

Find the log groups for the ECS services and related Lambda function. Review logs for:

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

![CloudWatch log groups](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/image9.png)

Expected result:

- No critical errors such as `CannotPullContainerError`.
- No database connection failure.
- No JWT verification failure.
- No SQS permission denied error.
