---
title: "Package and upload Lambda PostConfirmation"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Lambda PostConfirmation is invoked by Amazon Cognito after a user confirms an account. This function stores user information in Amazon RDS MySQL.

Keep using the same terminal window from the previous step and run the following commands there. This terminal session already contains the deployment variables:

```powershell
$REGION = "ap-southeast-1"
$ACCOUNT_ID = $(aws sts get-caller-identity --query Account --output text)
$ARTIFACT_BUCKET = "hashop-artifacts-$ACCOUNT_ID-$REGION"
```

Move to the Lambda source folder:

```powershell
cd D:\HaShop-release\cognito-post-confirmation-db
```

Install the required dependency:

```powershell
npm install mysql2
```

Compress the source code into `function.zip`:

```powershell
Compress-Archive `
  -Path .\index.js, .\node_modules, .\package.json, .\package-lock.json `
  -DestinationPath .\function.zip `
  -Force
```

Upload the package to the artifact bucket:

```powershell
aws s3 cp .\function.zip "s3://$ARTIFACT_BUCKET/lambda/post-confirmation/function.zip" --region $REGION
```

Verify that the file exists:

```powershell
aws s3 ls "s3://$ARTIFACT_BUCKET/lambda/post-confirmation/function.zip" --region $REGION
```

![Upload Lambda PostConfirmation package](/fcj-workshop-hungdung/images/5-Workshop/hashop-deployment/image2.png)
