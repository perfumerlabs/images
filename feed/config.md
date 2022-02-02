---
layout: default
parent: Feed
title: Configuration
nav_order: 3
---

Environment variables
=====================

- MS_TIMEZONE - Timezone of incoming or outcoming dates. Optional. Default is "Utc".
- FEED_COLLECTIONS - Create collections on startup (list through comma). Optional.

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
