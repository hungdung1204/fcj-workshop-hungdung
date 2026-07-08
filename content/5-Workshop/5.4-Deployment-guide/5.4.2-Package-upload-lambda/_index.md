---
title: "Package and upload Lambda PostConfirmation"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Lambda PostConfirmation is invoked by Amazon Cognito after a user confirms an account. This function stores user information in Amazon RDS MySQL.

Move to the Lambda source folder:

```powershell
cd D:\HaShop\hashop-infra\lambda\post-confirmation
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

![Upload Lambda PostConfirmation package](/images/5-Workshop/hashop-deployment/image2.png)
