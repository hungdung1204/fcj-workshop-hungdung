---
title: "Deployment Guide"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

This section provides the end-to-end deployment guide for HaShop. Each step is separated into its own page so the process is easier to follow and verify.

**5.4.1:** [Create the Lambda artifact S3 bucket](5.4.1-Create-lambda-artifact-bucket/)

**5.4.2:** [Package and upload Lambda PostConfirmation](5.4.2-Package-upload-lambda/)

**5.4.3:** [Create ECR repositories and push Docker images](5.4.3-Push-docker-images/)

**5.4.4:** [Clone the CloudFormation repository](5.4.4-Clone-cloudformation/)

**5.4.5:** [Deploy CloudFormation stacks](5.4.5-Deploy-cloudformation-stacks/)

**5.4.6:** [Prepare and publish the frontend](5.4.6-Prepare-frontend/)

> **Note:** Direct folder paths used in this guide, such as `mkdir D:\HaShop-release`, `cd D:\HaShop-release`, or `cd D:\HaShop-release\cognito-post-confirmation-db`, are examples for reference only. Replace them with the real path to the workspace folder on your own machine.
