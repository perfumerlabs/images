---
layout: default
parent: Queue
title: Installation
nav_order: 2
---

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e "QUEUE_WORKERS={\"my_worker\":1}" \
-v tarantool:/var/lib/tarantool \
-d images.perfumerlabs.com/dist/queue:v1.4.1
```
