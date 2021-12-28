---
layout: default
title: OTP
nav_order: 9
has_children: true
---

What is it
==========

This is a container providing one time passwords sending and verifying through sms and email.
This container must be set up in conjunction with [Queue](https://github.com/perfumerlabs/queue), [SMS](https://github.com/perfumerlabs/sms) and [Email](https://github.com/perfumerlabs/email).

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=otp_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/otp:v2.4.1
```

Environment variables
=====================

- OTP_QUEUE_URL - [Queue](https://github.com/perfumerlabs/queue) service URL. Default is `http://queue`.
- OTP_SMS_URL - [SMS](https://github.com/perfumerlabs/sms) service URL. Default is `http://sms`.
- OTP_SMS_WORKER - worker that handles sms queueing. Default is `sms`.
- OTP_CALL_URL - [CALL](https://github.com/perfumerlabs/sms) service URL. Default is `http://sms`.
- OTP_CALL_WORKER - worker that handles call queueing. Default is `sms`.
- OTP_EMAIL_URL - [Email](https://github.com/perfumerlabs/email) service URL. Default is `http://email`.
- OTP_EMAIL_WORKER - worker that handles email queueing. Default is `email`.
- OTP_TELEGRAM_URL - URL of [Telegram sending service](https://github.com/flxs/http-telegram-notify). Default is `http://telegram:8080`.
- OTP_TELEGRAM_APP_TOKEN - `APP_TOKEN` parameter of [Telegram sending service](https://github.com/flxs/http-telegram-notify). Required, if telegram OTPs are going to be used.
- OTP_TELEGRAM_WORKER - worker that handles telegram queueing. Default is `telegram`.
- DUMMY - If `true`, then no real interaction with sms/email/call/telegram/etc services are done. Default is `false`.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- PG_HOST - PostgreSQL connection host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- HTTP_AUTH_USERNAME - username for HTTP Basic Authorization. Optional.
- HTTP_AUTH_PASSWORD - password for HTTP Basic Authorization. Optional.

#### Steps to configure otp's on your project

1. Configure [Queue](https://github.com/perfumerlabs/queue), [SMS](https://github.com/perfumerlabs/sms) and/or [Email](https://github.com/perfumerlabs/email)
1. Configure this container. Note, that default settings suppose that services above are called `queue`, `sms` and `email` in your cluster.
1. Create OTPs via `POST /sms` or `POST /email`.
1. Check OTPs via `GET /sms/check` or `GET /email/check`

#### Telegram otp's

You can do same setup as above:

1. Create your Telegram Bot.
1. Run [1flx/http-telegram-notify](https://github.com/flxs/http-telegram-notify) docker container.
1. Set OTP_TELEGRAM_URL, OTP_TELEGRAM_APP_TOKEN, OTP_TELEGRAM_WORKER appropriately.
1. Create OTPs via `POST /telegram`.
1. Check OTPs via `GET /telegram/check`.

Also there more out-of-the-box functionality. Instead of manually call to OTP API you can publish ready-to-use endpoint for the Bot.

1. Configure bot webhook to URL - https://your-otp-host/telegram/webhook. Note that URL must be `https` and internet accessible.
1. When a user types anything to bot, webhook is sent to OTP and it generates new password and send it to bot.
1. Then user can copy the password and fill in the form on your application.
1. Then you can validate this password through `GET /target` endpoint and obtain user's CHAT_ID and save to your storage.
1. Use CHAT_ID then to notify user about something.

#### Authorization

If you use telegram otp's and publish OTP service to internet, then some access restriction is needed.
There support of HTTP Basic Authorization.
Fill in `HTTP_AUTH_USERNAME` and `HTTP_AUTH_PASSWORD` variables and authorization at nginx-level will be configured.

Beware, that HTTP Basic Authorization without HTTPS is insecure.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.

API Reference
=============

### Send sms OTP

`POST /sms`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- phone [string,required] - phone to send to, without "+".
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- message [string,required] - SMS message text to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "phone": "77011234567",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "phone_limit",
    "message": "Phone limit has been reached"
}
```

### Send call OTP

`POST /call`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- phone [string,required] - phone to send to, without "+".
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.

Request example:

```json
{
    "ip": "123.123.123.123",
    "phone": "77011234567",
    "password": "12345",
    "lifetime": 300
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "phone_limit",
    "message": "Phone limit has been reached"
}
```

### Send email OTP

`POST /email`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- email [string,required] - email to send to.
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- subject [string,required] - email subject to send.
- text [string,optional] - email text to send.
- html [string,optional] - email html content to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "email": "foobar@example.com",
    "password": "12345",
    "lifetime": 300,
    "subject": "Hello",
    "text": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "email_limit",
    "message": "Email limit has been reached"
}
```

### Send telegram OTP

`POST /telegram`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- chat_id [string,required] - telegram user CHAT_ID to send to.
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- message [string,required] - message to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "chat_id": "1234567890",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "email_limit",
    "message": "Email limit has been reached"
}
```

### Check sms OTP

`GET /sms/check`

Parameters (json):
- phone [string,required] - phone, without "+".
- password [string,required] - one time password.

Request example:

```json
{
    "phone": "77011234567",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Check email OTP

`GET /email/check`

Parameters (json):
- email [string,required] - email.
- password [string,required] - one time password.

Request example:

```json
{
    "email": "foobar@example.com",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Check telegram OTP

`GET /telegram/check`

Parameters (json):
- chat_id [string,required] - telegram user CHAT_ID.
- password [string,required] - one time password.

Request example:

```json
{
    "chat_id": "1234567890",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Set SMS limits

`POST /limit/sms`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- phones [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "phones": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

Response example:

```json
{
    "status": true
}
```

### Set CALL limits

`POST /limit/call`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- phones [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "phones": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

Response example:

```json
{
    "status": true
}
```

### Set email limits

`POST /limit/email`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- emails [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "emails": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

### Get last user credentials by a password

`GET /target`

Parameters (json):
- password [string,required] - password string.
- channel [string,required] - `sms`, `email`, `call` or `telegram`

Request example:

```json
{
    "password": "1234567",
    "channel": "telegram"
}
```

Response example:

```json
{
    "status": true,
    "content": {
        "target": "1234567890"
    }
}
```

### Get last password by a user credentials

`GET /password`

Parameters (json):
- target [string,required] - phone, email or telegram id.

Request example:

```json
{
    "target": "77011234567"
}
```

Response example:

```json
{
    "status": true,
    "content": {
        "password": "123456"
    }
}
```