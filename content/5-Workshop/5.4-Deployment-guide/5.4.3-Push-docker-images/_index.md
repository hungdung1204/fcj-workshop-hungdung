---
title: "Create ECR repositories and push Docker images"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Set variables for the target AWS account and ECR registry:

```powershell
$REGION = "ap-southeast-1"
$ACCOUNT_ID = $(aws sts get-caller-identity --query Account --output text)
$ECR = "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com"
```

Create the ECR repositories used by the microservices:

```powershell
$REPOS = @(
  "hashop/user-service",
  "hashop/product-service",
  "hashop/inventory-service",
  "hashop/cart-service",
  "hashop/order-service",
  "hashop/payment-service",
  "hashop/notification-service"
)

foreach ($repo in $REPOS) {
  aws ecr describe-repositories --repository-names $repo --region $REGION *> $null
  if ($LASTEXITCODE -ne 0) {
    aws ecr create-repository `
      --repository-name $repo `
      --image-scanning-configuration scanOnPush=true `
      --region $REGION
  }
}
```

Log in Docker to Amazon ECR:

```powershell
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR
```

If the terminal shows `Login Succeeded`, Docker can push images to ECR.

![Docker login to ECR](/images/5-Workshop/hashop-deployment/image3.png)

Download the tested container image release package:

[HaShop Docker images package](https://drive.google.com/file/d/1o9IrPV95KHFCW1tLL5gyHjRJ-FIiyEql/view?usp=drive_link)

Extract the package into `D:\HaShop-release\HaShop-docker-images`, then run:

```powershell
$IMAGE_DIR = "D:\HaShop-release\HaShop-docker-images"
$IMAGES = @(
  @{Repo="hashop/user-service"; File="user-service.tar"},
  @{Repo="hashop/product-service"; File="product-service.tar"},
  @{Repo="hashop/inventory-service"; File="inventory-service.tar"},
  @{Repo="hashop/cart-service"; File="cart-service.tar"},
  @{Repo="hashop/order-service"; File="order-service.tar"},
  @{Repo="hashop/payment-service"; File="payment-service.tar"},
  @{Repo="hashop/notification-service"; File="notification-service.tar"}
)

foreach ($img in $IMAGES) {
  $tarPath = Join-Path $IMAGE_DIR $img.File
  $loadOutput = docker load -i $tarPath
  $sourceImage = $null

  foreach ($line in $loadOutput) {
    if ($line -match "Loaded image:\s*(.+)$") {
      $sourceImage = $Matches[1].Trim()
    }
  }

  if (-not $sourceImage) {
    Write-Host "Cannot detect the image name after docker load. Run docker images to check." -ForegroundColor Red
    break
  }

  $targetImage = "$ECR/$($img.Repo):latest"
  docker tag $sourceImage $targetImage
  docker push $targetImage
}
```

If the image package is stored in another folder, update `$IMAGE_DIR` to the actual path.

![Load and push Docker images](/images/5-Workshop/hashop-deployment/image4.png)

![Docker images pushed to ECR](/images/5-Workshop/hashop-deployment/image5.png)
