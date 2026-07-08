---
title: "Region triển khai"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

HaShop sử dụng hai AWS Region:

| Region | Mục đích |
|---|---|
| `us-east-1` | Tạo AWS WAF Web ACL cho Amazon CloudFront. |
| `ap-southeast-1` | Triển khai toàn bộ hạ tầng chính của hệ thống. |

Khi AWS WAF được gắn với CloudFront, Web ACL phải dùng scope `CLOUDFRONT` và bắt buộc được tạo tại region `us-east-1`.
