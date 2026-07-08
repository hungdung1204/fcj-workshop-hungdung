---
title: "Chuẩn bị và publish frontend"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

Clone frontend repository từ thư mục release:

```powershell
cd D:\HaShop-release
git clone https://github.com/Leviethaidang/Front-end.git
cd Front-end
```

Sử dụng branch `main` vì đây là phiên bản ổn định đã được kiểm thử cho quy trình triển khai này.

Sau khi Stack 01 tạo thành công, mở phần outputs của stack và lấy:

- `FrontendBucketName`
- `FrontendCloudFrontDistributionId`

![Output của frontend stack](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image20.png)

Khai báo các biến triển khai frontend:

```powershell
$FRONTEND_BUCKET = "<FrontendBucketName>"
$REGION = "ap-southeast-1"
$FRONTEND_DISTRIBUTION_ID = "<FrontendCloudFrontDistributionId>"
```

Đồng bộ source frontend lên S3 frontend bucket:

```powershell
aws s3 sync "." "s3://$FRONTEND_BUCKET" `
  --delete `
  --region $REGION
```

Tạo CloudFront invalidation để người dùng nhận được phiên bản frontend mới nhất:

```powershell
aws cloudfront create-invalidation `
  --distribution-id $FRONTEND_DISTRIBUTION_ID `
  --paths "/*"
```

![Sync frontend lên S3](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image21.png)

![Tạo CloudFront invalidation](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image22.png)
