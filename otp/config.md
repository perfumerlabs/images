---
layout: default
parent: One Time Password
title: Configuration
nav_order: 3
---

Environment variables
=====================

- OTP_QUEUE_URL - [Queue](/images/queue) service URL. Default is `http://queue`.
- OTP_SMS_URL - [SMS](/images/sms) service URL. Default is `http://sms`.
- OTP_SMS_WORKER - worker that handles sms queueing. Default is `sms`.
- OTP_CALL_URL - [CALL](/images/sms) service URL. Default is `http://sms`.
- OTP_CALL_WORKER - worker that handles call queueing. Default is `sms`.
- OTP_EMAIL_URL - [Email](/images/email) service URL. Default is `http://email`.
- OTP_EMAIL_WORKER - worker that handles email queueing. Default is `email`.
- OTP_TELEGRAM_URL - URL of [Telegram sending service](https://github.com/flxs/http-telegram-notify). Default is `http://telegram:8080`.
- OTP_TELEGRAM_APP_TOKEN - `APP_TOKEN` parameter of [Telegram sending service](https://github.com/flxs/http-telegram-notify). Required, if telegram OTPs are going to be used.
- OTP_TELEGRAM_WORKER - worker that handles telegram queueing. Default is `telegram`.
- PG_HOST - PostgreSQL connection host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- HTTP_AUTH_USERNAME - username for HTTP Basic Authorization. Optional.
- HTTP_AUTH_PASSWORD - password for HTTP Basic Authorization. Optional.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- DUMMY - If `true`, then no real interaction with sms/email/call/telegram/etc services are done. Default is `false`.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
