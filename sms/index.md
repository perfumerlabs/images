---
layout: default
title: SMS
nav_order: 11
has_children: true
---

What is it
==========

This is a container providing API to send sms through external SMS providers.
Currently only [SMSC.RU](https://smsc.ru/) provider is supported.
It is recommended to set up this container to work in conjunction with [Queue](https://github.com/perfumerlabs/queue) container.

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e SMS_PROVIDER=smscru \
-e SMS_DUMMY=false \
-e SMSCRU_SENDER=sender \
-e SMSCRU_USERNAME=username \
-e SMSCRU_PASSWORD=password \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=sms_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/sms:v1.3.1
```

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

Supported providers for now
===========================

1. [smsc.ru](https://smsc.ru) - code "smscru"

Feel free to request support for any other sms providers.

API Reference
=============

### Send sms

`POST /sms`

Parameters (json):
- phones [array|string,required] - phones to send to, without "+".
- message [string,required] - message text to send.
- force [bool,optional] - if "true" ignores blacklisting. Default is false.
- type [string,optional] - "sms" or "call". Default is "sms".

Request example:

```json
{
    "phones": ["77011234567", "77070001234"],
    "message": "Hello, world!"
}
```

Response example:

```json
{
    "status": true
}
```

### Add phone to blacklist

`POST /blacklist`

Parameters (json):
- phone [string,required] - phone to add, without "+".

Request example:

```json
{
    "phone": "77011234567"
}
```

Response example:

```json
{
    "status": true
}
```

### Delete phone from blacklist

`DELETE /blacklist/{:phone}`

Parameters (url):
- phone [string,required] - phone to delete, without "+".

Request example:

```query
DELETE /blacklist/77011234567
```

Response has 404 status code, if not in the blacklist.

Response example, if in the blacklist:

```json
{
    "status": true
}
```

### Check if phone in blacklist

`GET /blacklist/{:phone}`

Parameters (url):
- phone [string,required] - phone to check, without "+".

Request example:

```query
GET /blacklist/77011234567
```

Response has 404 status code, if not in the blacklist.

Response example, if in the blacklist:

```json
{
    "status": true,
    "content": {
        "blacklist": {
            "phone": "77011234567",
            "created_at": "2020-09-01 00:00:00"
        }
    }
}
```