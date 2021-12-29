---
layout: default
parent: SMS
title: Configuration
nav_order: 3
---

Environment variables
=====================

- SMS_PROVIDER - Sms provider to use. Required. Currently the only value is "smscru".
- SMS_DUMMY - If this is "true", sms will not be sent, but response will be as sms was successfully sent. Useful in development configurations. Optional. Default is "false".
- SMSCRU_SENDER - [smsc.ru](https://smsc.ru) provider sender name for sms. Required, if this provider is used.
- SMSCRU_USERNAME - [smsc.ru](https://smsc.ru) provider login. Required, if this provider is used.
- SMSCRU_PASSWORD - [smsc.ru](https://smsc.ru) provider password. Required, if this provider is used.
- PG_HOST - PostgreSQL host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
