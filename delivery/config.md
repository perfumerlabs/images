---
layout: default
parent: Delivery
title: Configuration
nav_order: 3
---

Environment variables
=====================

- DELIVERY_QUEUE_URL - [Queue](https://github.com/perfumerlabs/queue) service URL. Required.
- DELIVERY_SMS_URL [SMS](https://github.com/perfumerlabs/sms) service URL. Required.
- DELIVERY_SMS_WORKER - worker that handles sms queueing. Required.
- DELIVERY_EMAIL_URL [Email](https://github.com/perfumerlabs/email) service URL. Required.
- DELIVERY_EMAIL_WORKER - worker that handles email queueing. Required.
- DELIVERY_FEED_URL [Feed](https://github.com/perfumerlabs/feed) service URL. Required.
- DELIVERY_FEED_WORKER - worker that handles feed queueing. Required.
- DELIVERY_URL - this service URL. Required.
- DELIVERY_FRACTION_WORKER - worker that handles delivery queueing. Required.
  DELIVERY_TIMEZONE - timezone

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will
be executed on container setup.
