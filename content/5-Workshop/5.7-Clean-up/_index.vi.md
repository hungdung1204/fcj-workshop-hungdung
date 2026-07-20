---
title: "Dọn dẹp tài nguyên"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Clean-up cần thực hiện sau khi hoàn thành lab để tránh phát sinh chi phí AWS không cần thiết. Nên xóa tài nguyên theo đúng thứ tự vì một số stack phụ thuộc vào tài nguyên được tạo bởi các stack trước đó.

**5.6.1:** [Xóa DB init stack](5.6.1-Delete-db-init-stack/)

**5.6.2:** [Xóa ECS services stack](5.6.2-Delete-ecs-services-stack/)

**5.6.3:** [Empty S3 bucket do foundation tạo](5.6.3-Empty-S3-buckets/)

**5.6.4:** [Xóa foundation stack](5.6.4-Delete-foundation-stack/)

**5.6.5:** [Xóa WAF stack](5.6.5-Delete-WAF-stack/)

**5.6.6:** [Xóa ECR repositories](5.6.6-Delete-ECR-repositories/)

**5.6.7:** [Xóa artifact và template bucket](5.6.7-Delete-artifact-buckets/)
