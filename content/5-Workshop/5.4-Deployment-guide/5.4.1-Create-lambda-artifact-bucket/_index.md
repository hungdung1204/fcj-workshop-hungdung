---
title: "Create the Lambda artifact S3 bucket"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

First, create a release workspace and clone the Lambda PostConfirmation repository.

```powershell
mkdir D:\HaShop-release
cd D:\HaShop-release
git clone https://github.com/Leviethaidang/cognito-post-confirmation-db.git
```

Set the deployment variables:

```powershell
$REGION = "ap-southeast-1"
$ACCOUNT_ID = $(aws sts get-caller-identity --query Account --output text)
$ARTIFACT_BUCKET = "hashop-artifacts-$ACCOUNT_ID-$REGION"
```

Create the artifact bucket:

```powershell
aws s3api create-bucket `
  --bucket $ARTIFACT_BUCKET `
  --region $REGION `
  --create-bucket-configuration LocationConstraint=$REGION
```

Enable Block Public Access:

```powershell
aws s3api put-public-access-block `
  --bucket $ARTIFACT_BUCKET `
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

Enable server-side encryption:

```powershell
$SSE_CONFIG = "$env:TEMP\hashop-s3-sse.json"
@'
{"Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}}]}
'@ | Set-Content -Path $SSE_CONFIG -Encoding ascii

aws s3api put-bucket-encryption `
  --bucket $ARTIFACT_BUCKET `
  --server-side-encryption-configuration "file://$SSE_CONFIG"
```

![Create Lambda artifact bucket](/images/5-Workshop/hashop-deployment/image1.png)
