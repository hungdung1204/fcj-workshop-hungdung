---
title: "Package và upload Lambda PostConfirmation"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Lambda PostConfirmation được Amazon Cognito gọi sau khi người dùng xác nhận tài khoản. Lambda này ghi thông tin user vào Amazon RDS MySQL.

Di chuyển vào thư mục source của Lambda:

```powershell
cd D:\HaShop\hashop-infra\lambda\post-confirmation
```

Cài dependency cần thiết:

```powershell
npm install mysql2
```

Nén source code thành `function.zip`:

```powershell
Compress-Archive `
  -Path .\index.js, .\node_modules, .\package.json, .\package-lock.json `
  -DestinationPath .\function.zip `
  -Force
```

Upload package lên artifact bucket:

```powershell
aws s3 cp .\function.zip "s3://$ARTIFACT_BUCKET/lambda/post-confirmation/function.zip" --region $REGION
```

Kiểm tra file đã được upload:

```powershell
aws s3 ls "s3://$ARTIFACT_BUCKET/lambda/post-confirmation/function.zip" --region $REGION
```

![Upload Lambda PostConfirmation package](/fcj-workshop-hungdung/images/5-Workshop/hashop-deployment/image2.png)
