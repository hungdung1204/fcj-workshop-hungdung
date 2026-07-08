---
title: "Access the frontend"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

Open a browser and access the CloudFront domain:

```text
https://<FrontendCloudFrontDomain>
```

`<FrontendCloudFrontDomain>` is taken from the Outputs tab of the `cf-hashop-foundation` stack.

Expected result:

- The HaShop home page loads successfully.
- Static assets are served through CloudFront.
- The page does not show a browser-side loading error.

![HaShop frontend loaded successfully](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/image1.png)
