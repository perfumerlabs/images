---
layout: default
parent: Kassa24 Payments
title: Configuration
nav_order: 3
---

Environment variables
=====================

- KASSA24_CHECK_URL - URL to send CHECK command requests. Optional.
- KASSA24_WEBHOOK_URL - URL to send webhooks after successful payment. Optional.
- HTTP_AUTH_USERNAME - HTTP Basic Authentication username. Optional.
- HTTP_AUTH_PASSWORD - HTTP Basic Authentication password. Optional.
- MS_TIMEZONE - Timezone of your location. It affects to webhook request sent dates.  Optional. Default is "UTC".
- DEBUG - If this is "true", extra logging information is printed to container STDOUT. Optional. Default is "false".

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
