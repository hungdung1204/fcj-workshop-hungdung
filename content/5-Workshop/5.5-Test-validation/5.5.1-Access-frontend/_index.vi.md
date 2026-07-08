---
title: "Truy cập frontend"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

Mở trình duyệt và truy cập CloudFront domain:

```text
https://<FrontendCloudFrontDomain>
```

`<FrontendCloudFrontDomain>` được lấy từ tab Outputs của stack `cf-hashop-foundation`.

Kết quả mong đợi:

- Trang chủ HaShop hiển thị thành công.
- Static asset được phân phối thông qua CloudFront.
- Trình duyệt không hiển thị lỗi tải trang.

![Trang chủ HaShop hiển thị thành công](/images/5-Workshop/hashop-test-cleanup/image1.png)
