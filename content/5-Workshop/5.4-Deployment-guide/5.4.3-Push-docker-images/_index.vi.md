---
title: "Tạo ECR repository và push Docker image"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Khai báo biến cho AWS account và ECR registry:

```powershell
$REGION = "ap-southeast-1"
$ACCOUNT_ID = $(aws sts get-caller-identity --query Account --output text)
$ECR = "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com"
```

Tạo các ECR repository dùng cho microservice:

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

Login Docker vào Amazon ECR:

```powershell
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR
```

Nếu terminal hiển thị `Login Succeeded` thì Docker đã có thể push image lên ECR.

![Docker login vào ECR](/images/5-Workshop/hashop-deployment/image3.png)

Tải gói container image đã kiểm thử:

[HaShop Docker images package](https://drive.google.com/file/d/1o9IrPV95KHFCW1tLL5gyHjRJ-FIiyEql/view?usp=drive_link)

Giải nén vào `D:\HaShop-release\HaShop-docker-images`, sau đó chạy:

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
    Write-Host "Không tìm được tên image sau khi docker load. Hãy chạy docker images để kiểm tra." -ForegroundColor Red
    break
  }

  $targetImage = "$ECR/$($img.Repo):latest"
  docker tag $sourceImage $targetImage
  docker push $targetImage
}
```

Nếu gói image được lưu ở vị trí khác, cần thay `$IMAGE_DIR` bằng đường dẫn thực tế trên máy.

![Load và push Docker image](/images/5-Workshop/hashop-deployment/image4.png)

![Docker image đã push lên ECR](/images/5-Workshop/hashop-deployment/image5.png)
