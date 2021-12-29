---
layout: default
parent: Upload
title: Cache
nav_order: 2
---

Uploader places cached images thumbnails to `/opt/upload/web/cache` directory in the container.
Over time this directory may become very big.
If you want to clear this directory you can just change corresponded volume to new one and remove old directory.

Another option is `CLEAR_CACHE_PERIOD` parameter.
This parameter is always set and equals to 5184000 seconds (2 months) by default.
Uploader has special daemon which permanently scans cache directory and removes all the files, which older than this setting.
Increase or decrease this value for your needs.
Daemon does nothing with original files, only cached.