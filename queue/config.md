---
layout: default
parent: Queue
title: Configuration
nav_order: 3
---

Environment variables
=====================

- QUEUE_WORKERS - list of workers in json format (see below). Required.
- QUEUE_DEFAULT_TIMEOUT - default timeout in seconds, when queue worker requests backend. Optional. Default value is 30.
- TARANTOOL_URI - Tarantool URI to connect, if external instance of Tarantool is used. Optional. Default value is "tcp://127.0.0.1:3301".
- DEBUG - if "true", print more debug information. Optional. Default value is "false".
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 60.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- PHP_MEMORY_LIMIT - memory_limit option in php.ini. Optional. The default value is 128M.

Volumes
=======

- /var/lib/tarantool - Tarantool data directory.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
