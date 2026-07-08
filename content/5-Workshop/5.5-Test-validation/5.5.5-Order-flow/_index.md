---
title: "Validate the order flow"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

Run the following order scenarios on the website:

1. Admin creates category, size, color, product, and inventory.
2. Customer adds a product to the cart.
3. Customer checks out with COD.
4. Customer checks out with the MOMO/BANK demo payment method.
5. Check the order status.

Expected result:

- COD orders are confirmed directly.
- MOMO/BANK orders move to the pending payment state.
- `payment-worker` processes messages from SQS.
- `order-worker` updates order status after payment results are available.

![Order checkout flow](/images/5-Workshop/hashop-test-cleanup/image7.png)

![Order status validation](/images/5-Workshop/hashop-test-cleanup/image8.png)
