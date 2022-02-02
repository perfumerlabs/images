---
layout: default
parent: PostgreSQL Backups Tester
title: Configuration
nav_order: 3
---

Environment variables
=====================

- DUMPS_DIR - directory in the container with dumps (not in the host!). Required.
- RESTORE_HOST - PostgreSQL host to restore dumps to. Required. DO NOT USE PRODUCTION POSTGRESQL INSTANCE.
- RESTORE_PORT - PostgreSQL port to restore dumps to. Default value is 5432.
- RESTORE_USER - PostgreSQL user name to restore dumps to. Required.
- RESTORE_PASSWORD - PostgreSQL user password to restore dumps to. Required.

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
