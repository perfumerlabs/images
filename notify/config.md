---
layout: default
parent: Notifications Center
title: Configuration
nav_order: 3
---

Environment variables
=====================

- NOTIFY_QUEUE_URL - [Queue](/images/queue) service URL. Default is `http://queue`.
- NOTIFY_SMS_URL - [SMS](/images/sms) service URL. Default is `http://sms`.
- NOTIFY_SMS_WORKER - worker that handles sms queueing. Default is `sms`.
- NOTIFY_EMAIL_URL - [Email](/images/email) service URL. Default is `http://email`.
- NOTIFY_EMAIL_WORKER - worker that handles email queueing. Default is `email`.
- NOTIFY_FEED_URL - [Feed](/images/feed) service URL. Default is `http://feed`.
- NOTIFY_FEED_WORKER - worker that handles feed queueing. Default is `feed`.

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
