---
layout: default
parent: Email
title: Installation
nav_order: 2
---

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e EMAIL_FROM=noreply@example.com \
-e SMTP_HOST=smtp \
-e SMTP_PORT=587 \
-e SMTP_USERNAME=username \
-e SMTP_PASSWORD=password \
-e SMTP_ENCRYPTION=tls \
-d images.perfumerlabs.com/dist/email:v1.2.1
```