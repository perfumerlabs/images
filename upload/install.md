---
layout: default
parent: Upload
title: Installation
nav_order: 3
---

Installation
============

Upload uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs Upload.

```bash
docker run \
-e UPLOAD_HOST=example.com \
-e PG_REAL_HOST=db \
-e PG_HOST=db \
-e PG_DATABASE=database \
-e PG_USER=user \
-e PG_PASSWORD=password \
-v files:/opt/upload/files \
-v cache:/opt/upload/web/cache \
-d images.perfumerlabs.com/dist/upload:v2.4.2
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
  upload:
    image: images.perfumerlabs.com/dist/upload:v2.4.2
    environment:
      UPLOAD_HOST: example.com
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: sms
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
    volumes:
      - /custom/mount/files:/opt/upload/files
      - /custom/mount/cache:/opt/upload/web/cache
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/upload/config) for parameters description.
