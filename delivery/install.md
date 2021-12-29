---
layout: default
parent: Delivery
title: Installation
nav_order: 2
---

Installation
============

Delivery uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-p 5432:5432/tcp \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs Delivery.

```bash
docker run \
-p 80:80/tcp \
-e DELIVERY_QUEUE_URL="http://queue" \
-e DELIVERY_SMS_URL="http://sms" \
-e DELIVERY_SMS_WORKER=sms \
-e DELIVERY_EMAIL_URL="http://email" \
-e DELIVERY_EMAIL_WORKER=email \
-e DELIVERY_FEED_URL="http://feed" \
-e DELIVERY_FEED_WORKER=feed \
-e DELIVERY_URL="http://delivery" \
-e DELIVERY_FRACTION_WORKER=delivery \
-e PG_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=delivery_db \
-e PG_SCHEMA=public \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/delivery:v1.1.0
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
  delivery:
    image: images.perfumerlabs.com/dist/delivery:v1.1.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: delivery
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/delivery/config) for parameters description.
