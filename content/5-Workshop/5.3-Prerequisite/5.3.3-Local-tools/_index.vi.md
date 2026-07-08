---
title: "Công cụ cục bộ"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Máy dùng để triển khai cần cài đặt các công cụ sau:

- Git
- Node.js và npm
- Docker Desktop
- AWS CLI v2
- PowerShell

Sau khi cài đặt, kiểm tra khả năng truy cập AWS từ command line:

```powershell
aws sts get-caller-identity
```

Lệnh này cần trả về AWS account ID, ARN của user hoặc role hiện tại, và user ID.
