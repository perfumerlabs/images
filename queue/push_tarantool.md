---
layout: default
parent: Queue
title: Work With Tarantool
nav_order: 6
---

### Pushing Tasks to Tarantool

You can push and pull tasks directly to\from Tarantool.

Configure needed collections via `QUEUE_WORKERS` parameter.
Don't forget to set zero workers for them:

```bash
docker run \
-e "QUEUE_WORKERS={\"my_worker\":0}" \
-d images.perfumerlabs.com/dist/queue:v1.5.0
```

After that container will create `my_worker` collection in Tarantool and no daemons listening tasks.
Then you can push and pull tasks with your own code on 3301 port.
See [Tarantool queue documentation](https://github.com/tarantool/queue) how to do it for your language.