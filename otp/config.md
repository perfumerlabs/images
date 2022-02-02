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
- HTTP_AUTH_USERNAME - username for HTTP Basic Authorization. Optional.
- HTTP_AUTH_PASSWORD - password for HTTP Basic Authorization. Optional.
- DUMMY - If `true`, then no real interaction with sms/email/call/telegram/etc services are done. Default is `false`.

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
