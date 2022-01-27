---
layout: default
parent: Request Catcher
title: Installation
nav_order: 2
---

Installation
============

Request Catcher does not require any dependencies:

```bash
docker run \
-d images.perfumerlabs.com/dist/request-catcher:v2.0.0
```

Environment variables
=====================

This image does not have environment parameters.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
