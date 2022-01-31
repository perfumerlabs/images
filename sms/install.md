---
layout: default
parent: SMS Gateways
title: Installation
nav_order: 2
---

Installation
============

SMS uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs SMS.

```bash
docker run \
-e SMS_PROVIDER=smscru \
-e SMSCRU_SENDER=sender \
-e SMSCRU_USERNAME=username \
-e SMSCRU_PASSWORD=password \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=sms_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/sms:v2.1.0
```

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
  sms:
    image: images.perfumerlabs.com/dist/sms:v2.1.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: sms
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      SMS_PROVIDER: smscru
      SMSCRU_SENDER: sender # your smscru account sender name (specified in their cabinet)
      SMSCRU_USERNAME: smscru_username # your smscru account username
      SMSCRU_PASSWORD: smscru_password # your smscru account password
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/sms/config) for parameters description.

Queueing
--------

Lets add queueing to our service. We use our ready-to-use [Queue](/images/queue) container.
With next command we create Queue container with 1 worker sending emails (you can set any number).

```bash
docker run \
-e "QUEUE_WORKERS={\"sms\":1}" \
-v tarantool:/var/lib/tarantool \
-d images.perfumerlabs.com/dist/queue:v1.4.1
```

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  queue:
    image: images.perfumerlabs.com/dist/queue:v1.4.1
    environment:
      QUEUE_WORKERS: "{\"sms\":1}"
    volumes:
      - tarantool:/var/lib/tarantool
  sms:
    image: images.perfumerlabs.com/dist/sms:v2.1.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: sms
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      SMS_PROVIDER: smscru
      SMSCRU_SENDER: sender # your smscru account sender name (specified in their cabinet)
      SMSCRU_USERNAME: smscru_username # your smscru account username
      SMSCRU_PASSWORD: smscru_password # your smscru account password
```

Now, instead of sending sms directly to SMS container we send request to Queue container.
And it then requests SMS container in the background by itself.
Typical request can be:

```
POST http://queue/task
```

```json
{
    "worker": "sms",
    "url": "http://sms/sms",
    "method": "post",
    "json": {
        "phones": "71234567890",
        "message": "Hi"
    }
}
```

Refer to [Queue](/images/queue) for detailed options.
