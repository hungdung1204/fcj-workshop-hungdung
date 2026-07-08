---
title: "Clone CloudFormation repository"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

Quay lại thư mục release và clone repository chứa CloudFormation template:

```powershell
cd D:\HaShop-release
git clone https://github.com/Leviethaidang/cloudformation.git
```

Repository này chứa các template dùng ở bước tiếp theo:

- `00-cf-hashop-global-waf.yaml`
- `01-cf-hashop-foundation.yaml`
- `02-cf-hashop-ecs-services.yaml`
- `03-cf-hashop-db-init.yaml`

![Clone CloudFormation repository](/images/5-Workshop/hashop-deployment/image6.png)
