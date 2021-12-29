---
layout: default
parent: OTP
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

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs OTP.

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
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/otp/config) for parameters description.
