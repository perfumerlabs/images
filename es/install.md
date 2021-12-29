---
layout: default
parent: ES
title: Installation
nav_order: 2
---

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e ES_HOST=elasticsearch \
-e ES_PORT=9200 \
-d images.perfumerlabs.com/dist/es:v1.1.2
```
