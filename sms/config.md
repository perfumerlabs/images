---
layout: default
parent: SMS Gateways
title: Configuration
nav_order: 3
---

Environment variables
=====================

- SMS_PROVIDER - Default sms provider to use. Required. "smscru", "twilio", "epochtaru", "mobizonkz" values are acceptable.
- TWILIO_SID - [twilio.com](https://twilio.com) providers SID parameter. Required, if this provider is used.
- TWILIO_TOKEN - [twilio.com](https://twilio.com) providers Token parameter. Required, if this provider is used.
- TWILIO_PHONE - [twilio.com](https://twilio.com) providers sender phone number. Required, if this provider is used.
- SMSCRU_SENDER - [smsc.ru](https://smsc.ru) provider sender name for sms. Required, if this provider is used.
- SMSCRU_USERNAME - [smsc.ru](https://smsc.ru) provider login. Required, if this provider is used.
- SMSCRU_PASSWORD - [smsc.ru](https://smsc.ru) provider password. Required, if this provider is used.
- EPOCHTARU_SENDER - [epochta.ru](https://www.epochta.ru/) provider sender name for sms. Required, if this provider is used.
- EPOCHTARU_USERNAME - [epochta.ru](https://www.epochta.ru/) provider login. Required, if this provider is used.
- EPOCHTARU_PASSWORD - [epochta.ru](https://www.epochta.ru/) provider password. Required, if this provider is used.
- MOBIZONKZ_API_KEY - [mobizon.kz](https://mobizon.kz/) API key from personal cabinet. Required, if this provider is used.
- PG_HOST - PostgreSQL host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- SMS_DUMMY - If this is "true", sms will not be sent, but response will be as sms was successfully sent. Useful in development configurations. Optional. Default is "false".

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
