---
layout: default
parent: Kassa24 Payments
title: Installation
nav_order: 2
---

Installation
============

Kassa24 Payments uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs Kassa24 Payments.

```bash
docker run \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=kassa24 \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e KASSA24_CHECK_URL=http://example.com/my-check-url \
-e KASSA24_WEBHOOK_URL=http://example.com/my-webhook-url \
-e HTTP_AUTH_USERNAME=http_username \
-e HTTP_AUTH_PASSWORD=http_password \
-d images.perfumerlabs.com/dist/kassa24:v1.2.0
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
  kassa24:
    image: images.perfumerlabs.com/kassa24:v1.1.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: kassa24
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      KASSA24_CHECK_URL: http://example.com/my-check-url
      KASSA24_WEBHOOK_URL: http://example.com/my-webhook-url
      HTTP_AUTH_USERNAME: http_username
      HTTP_AUTH_PASSWORD: http_password
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/kassa24/config) for parameters description.
