---
title: "Deploy CloudFormation stacks"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

## Stack 0: Global WAF

Switch the AWS Console region to **United States (N. Virginia) `us-east-1`**. Open CloudFormation and create a stack with an existing template. Upload `00-cf-hashop-global-waf.yaml`.

![Upload WAF template](/images/5-Workshop/hashop-deployment/image7.png)

Choose **Next**.

![CloudFormation WAF stack details](/images/5-Workshop/hashop-deployment/image8.png)

Set the stack name to `HaShop-WAF`, then continue with **Next**.

![Set WAF stack name](/images/5-Workshop/hashop-deployment/image9.png)

Review the configuration and choose **Submit**.

![Submit WAF stack](/images/5-Workshop/hashop-deployment/image10.png)

After the stack is created, open the **Outputs** tab and copy `FrontendWebAclArn`. This ARN will be used when deploying the foundation stack.

## Stack 01: Foundation

Switch the AWS Console region back to **Singapore `ap-southeast-1`**. Create another CloudFormation stack and upload `01-cf-hashop-foundation.yaml`.

![Upload foundation template](/images/5-Workshop/hashop-deployment/image11.png)

Set the stack name exactly to `cf-hashop-foundation`.

![Set foundation stack name](/images/5-Workshop/hashop-deployment/image12.png)

Provide the required parameters:

- `DbPassword`: choose a database password.
- `InternalApiKey`: choose a 16-character internal API key.
- `AdminEmail`: enter your email address.
- `SesFromEmail`: enter the SES verified sender email address.

![Foundation stack parameters](/images/5-Workshop/hashop-deployment/image13.png)

Set the Lambda artifact parameters:

- `LambdaCodeS3Bucket`: `hashop-artifacts-<ACCOUNT_ID>-ap-southeast-1`
- `LambdaCodeS3Key`: `lambda/post-confirmation/function.zip`
- `FrontendWebAclArn`: the ARN copied from Stack 0

![Foundation Lambda and WAF parameters](/images/5-Workshop/hashop-deployment/image14.png)

Continue to the review page, acknowledge that CloudFormation may create IAM resources with custom names, and submit the stack.

If ECS tasks fail because IAM roles have not propagated yet, delete the failed stack and run it again.

## Stack 02: ECS services

Wait until Stack 01 finishes. It can take around 20 minutes. Then create a new CloudFormation stack using `02-cf-hashop-ecs-services.yaml`. Set the stack name to `cf-hashop-ecs`.

![Create ECS services stack](/images/5-Workshop/hashop-deployment/image15.png)

In the container image parameters, replace `<ACCOUNT_ID>` with the real AWS account ID.

![ECS container image parameters](/images/5-Workshop/hashop-deployment/image16.png)

## Stack 03: Database initialization

Stack 03 can be started after Stack 02 is submitted. Create a new CloudFormation stack using `03-cf-hashop-db-init.yaml`.

![Upload database initialization template](/images/5-Workshop/hashop-deployment/image17.png)

Set a stack name for the database initialization stack.

![Database initialization stack name](/images/5-Workshop/hashop-deployment/image18.png)

Review the stack configuration.

![Database initialization review](/images/5-Workshop/hashop-deployment/image19.png)

Acknowledge that CloudFormation may create IAM resources with custom names, then submit the stack.
