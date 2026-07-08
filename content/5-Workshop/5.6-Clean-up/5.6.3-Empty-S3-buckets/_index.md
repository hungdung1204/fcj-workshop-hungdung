---
title: "Empty S3 buckets created by the foundation stack"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

Before deleting the foundation stack, empty the S3 buckets created by the stack. CloudFormation cannot delete non-empty buckets automatically.

Expected result:

- The target resource is deleted successfully.
- No dependent stack remains in a failed deletion state.
- The AWS account no longer keeps resources that can generate lab costs.

![Empty S3 buckets created by the foundation stack](/fcj-workshop-haidang/images/5-Workshop/hashop-test-cleanup/image13.png)
