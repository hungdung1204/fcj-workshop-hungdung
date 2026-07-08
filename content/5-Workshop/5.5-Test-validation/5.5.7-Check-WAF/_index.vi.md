---
title: "Kiểm tra AWS WAF"
date: 2026-07-04
weight: 7
chapter: false
pre: " <b> 5.5.7. </b> "
---

Trên AWS Console, mở:

```text
AWS WAF -> Web ACLs -> Global / CloudFront -> HaShop Web ACL
```

Kiểm tra các phần sau:

- Allowed requests
- Blocked requests
- Sampled requests
- Rule metrics

![Metric của AWS WAF](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/image10.png)

Kết quả mong đợi:

- WAF Web ACL đã được associate với CloudFront.
- Metric request đã có dữ liệu.
- Có thể kiểm tra sampled requests để xác thực hoạt động của WAF.
