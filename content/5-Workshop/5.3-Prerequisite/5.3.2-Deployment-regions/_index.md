---
title: "Deployment regions"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

HaShop uses two AWS regions:

| Region | Purpose |
|---|---|
| `us-east-1` | Create the AWS WAF Web ACL for Amazon CloudFront. |
| `ap-southeast-1` | Deploy the main infrastructure of the system. |

When AWS WAF is associated with CloudFront, the Web ACL must use the `CLOUDFRONT` scope and must be created in `us-east-1`.
