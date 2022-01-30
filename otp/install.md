---
layout: default
parent: One time Password
title: Installation
nav_order: 2
---

Installation
============

OTP uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-p 5432:5432/tcp \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

OTP relies on [Queue](/images/queue), [SMS](/images/sms) and [Email](/images/email) images.
Refer to their installation chapters.

Also if you use Telegram based OTPs, then refer to [1flx/http-telegram-notify](https://github.com/flxs/http-telegram-notify).

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  postgres:
    image: postgres
    environment:
      POSTGRES_PASSWORD: mysecretpassword
    volumes:
      - /custom/mount:/var/lib/postgresql/data
  otp:
    image: images.perfumerlabs.com/dist/otp:v2.4.1
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: otp
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      OTP_TELEGRAM_APP_TOKEN: APP_TOKEN string set to telegram container
      HTTP_AUTH_USERNAME: optional-http-basic-username
      HTTP_AUTH_PASSWORD: optional-http-basic-password
    depends_on:
      postgres:
        condition: service_started
  sms:
    image: images.perfumerlabs.com/dist/sms:v2.0.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: sms
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      # example with smsc.ru provider credentials
      SMS_PROVIDER: smscru
      SMSCRU_SENDER: sender # your smscru account sender name (specified in their cabinet)
      SMSCRU_USERNAME: smscru_username # your smscru account username
      SMSCRU_PASSWORD: smscru_password # your smscru account password
    depends_on:
      postgres:
        condition: service_started
  email:
    image: images.perfumerlabs.com/dist/email:v1.3.0
    environment:
      # example of sendpulse.com credentials
      EMAIL_FROM: noreply@your-domain.com
      SMTP_HOST: smtp-pulse.com
      SMTP_PORT: 465
      SMTP_USERNAME: sendpulse-account
      SMTP_PASSWORD: sendpulse-password
      SMTP_ENCRYPTION: ssl
  telegram:
    image: 1flx/http-telegram-notify
    environment:
      APP_TOKEN: your-app-token # come up with custom string
      TELEGRAM_TOKEN: your-telegram-bot-token
  queue:
    image: images.perfumerlabs.com/dist/queue:v1.4.2
    environment:
      # set more workers if needed
      QUEUE_WORKERS: "{\"sms\":1,\"email\":1,\"call\":1,\"telegram\":1}"
    volumes:
      - tarantool:/var/lib/tarantool
```

Note, that OTP relies on services `sms`, `email`, `queue` and `telegram` to have these names.
If you want to customize them, refer to [configuration page](/images/otp/config) for parameters description.
