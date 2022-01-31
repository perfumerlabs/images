---
layout: default
parent: Queue
title: Installation
nav_order: 2
---

Installation
============

To install Queue you can use command below:

```bash
docker run \
-e "QUEUE_WORKERS={\"my_worker\":1}" \
-v tarantool:/var/lib/tarantool \
-d images.perfumerlabs.com/dist/queue:v1.4.2
```

Tarantool is bundled inside so you don't have to configure it separately.
Make sure to provide `tarantool` volume to persist tasks database.

If you want to use dedicated Tarantool instance for some reason (not recommended), you can provide `TARANTOOL_URI` variable:

```bash
docker run \
-e "QUEUE_WORKERS={\"my_worker\":1}" \
-e "TARANTOOL_URI=tcp://my-tarantool-instance:3301" \
-d images.perfumerlabs.com/dist/queue:v1.4.2
```

Note, that in this case you have to configure tarantool queue settings by yourself:

- Install `queue` module `apt-get install tarantool-queue`
- Launch tarantool with command `/usr/bin/tarantool /etc/tarantool/instances.enabled/queue.lua`, where `queue.lua` can be:

```
#!/usr/bin/env tarantool

box.cfg {
    listen           = '0.0.0.0:3301';
    slab_alloc_arena = 1;
    wal_dir          = "/var/lib/tarantool";
    snap_dir         = "/var/lib/tarantool";
    vinyl_dir        = "/var/lib/tarantool";
    username         = "tarantool";
}
box.schema.user.grant('guest', 'read,write,execute', 'universe', nil, {if_not_exists = true})

queue = require('queue')
queue.start()
queue.create_tube('my_worker1', 'fifottl', { if_not_exists=true })
queue.create_tube('my_worker2', 'fifottl', { if_not_exists=true })
```

Make sure to fix `my_worker1` and `my_worker2` to names of your workers.