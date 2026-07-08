---
title: "Admin login and authorization"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

Use the default admin account to sign in:

```text
Email: admin@example.com
Password: Letmein123!!!
```

After signing in, open the admin functions such as product management.

Expected result:

- The admin account can sign in successfully.
- Admin-only pages can be opened after authentication.
- Requests that require authorization are sent with a valid token.
- The user is not redirected back to the login page unexpectedly.
