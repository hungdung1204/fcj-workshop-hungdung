---
title: "Check AWS WAF"
date: 2026-07-04
weight: 7
chapter: false
pre: " <b> 5.5.7. </b> "
---

In the AWS Console, open:

```text
AWS WAF -> Web ACLs -> Global / CloudFront -> HaShop Web ACL
```

Check the following WAF views:

- Allowed requests
- Blocked requests
- Sampled requests
- Rule metrics

![AWS WAF metrics](/images/5-Workshop/hashop-test-cleanup/image10.png)

Expected result:

- The WAF Web ACL is associated with CloudFront.
- Request metrics are available.
- Sampled requests can be inspected for validation.
