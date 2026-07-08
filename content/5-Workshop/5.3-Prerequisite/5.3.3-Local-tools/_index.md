---
title: "Local tools"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Install the following tools on the deployment machine:

- Git
- Node.js and npm
- Docker Desktop
- AWS CLI v2
- PowerShell

After installation, verify that the command line can access AWS:

```powershell
aws sts get-caller-identity
```

The command should return the current AWS account ID, user or role ARN, and user ID.
