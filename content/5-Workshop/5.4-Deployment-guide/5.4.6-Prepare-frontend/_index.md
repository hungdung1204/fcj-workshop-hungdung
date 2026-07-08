---
title: "Prepare and publish the frontend"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

Clone the frontend repository from the release folder:

```powershell
cd D:\HaShop-release
git clone https://github.com/Leviethaidang/Front-end.git
cd Front-end
```

Use the `main` branch because it is the stable version that has been tested for this deployment.

After Stack 01 is created successfully, open the stack outputs and copy:

- `FrontendBucketName`
- `FrontendCloudFrontDistributionId`

![Frontend stack outputs](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image20.png)

Set the frontend deployment variables:

```powershell
$FRONTEND_BUCKET = "<FrontendBucketName>"
$REGION = "ap-southeast-1"
$FRONTEND_DISTRIBUTION_ID = "<FrontendCloudFrontDistributionId>"
```

Sync the frontend source to the S3 frontend bucket:

```powershell
aws s3 sync "." "s3://$FRONTEND_BUCKET" `
  --delete `
  --region $REGION
```

Create a CloudFront invalidation so users receive the latest frontend files:

```powershell
aws cloudfront create-invalidation `
  --distribution-id $FRONTEND_DISTRIBUTION_ID `
  --paths "/*"
```

![Sync frontend to S3](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image21.png)

![Create CloudFront invalidation](/fcj-workshop-haidang/images/5-Workshop/hashop-deployment/image22.png)
