---
title: "Clean-up"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Clean-up must be performed after the lab to avoid unnecessary AWS costs. Delete resources in the correct order because some stacks depend on resources created by earlier stacks.

**5.6.1:** [Delete the DB init stack](5.6.1-Delete-db-init-stack/)

**5.6.2:** [Delete the ECS services stack](5.6.2-Delete-ecs-services-stack/)

**5.6.3:** [Empty S3 buckets created by the foundation stack](5.6.3-Empty-S3-buckets/)

**5.6.4:** [Delete the foundation stack](5.6.4-Delete-foundation-stack/)

**5.6.5:** [Delete the WAF stack](5.6.5-Delete-WAF-stack/)

**5.6.6:** [Delete ECR repositories](5.6.6-Delete-ECR-repositories/)

**5.6.7:** [Delete artifact and template buckets](5.6.7-Delete-artifact-buckets/)
