---
layout: default
parent: Upload
title: Scaling
nav_order: 1
---

Even if you dedicate 2-3 TB HDD server for file server, there will be moment, when disk space will be exhausted.
Uploader already has scaling mechanism.

Provide `UPLOAD_DIGEST_PREFIX` parameter to container setup.

```
docker run -ti \
...
-e UPLOAD_DIGEST_PREFIX=abcde \
...
```

This means that after file uploaded every digest will be prefixed with fixed string "abcde".
So you know that digests beginning with this "abcde" belong to this particular server.
Although `UPLOAD_DIGEST_PREFIX` is optional, it is very recommended to set it to any uploader instance.

Then you can configure rules on you loadbalancer (nginx, HaProxy, ingress) to route "abcde" to proper server:

- Route "https://upload.example.com/file/abcde*" urls to old uploader instance.
- Route all other "https://upload.example.com/file" urls to new uploader instance.