---
layout: default
parent: Ncanode
title: Installation
nav_order: 2
---

Installation
============

Ncanode uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-p 5432:5432/tcp \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Then you have to install [Ncanode](https://ncanode.kz) server.
Author of that project does not support docker image, though there are a number of community prebuilt images.
Search on the Docker Hub or use image below.

```bash
docker run \
-p 14579:14579/tcp \
-d ozayev/ncanode2.3.0_kc_0.6.1_ca:latest
```

Then this command installs Ncanode.

```bash
docker run \
-p 80:80/tcp \
-e NCANODE_REMOTE_URL=http://ncanode-origin:14579 \
-e NCANODE_KEY=/path/to/key \
-e NCANODE_PWD=123456 \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=ncanode_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/ncanode:v3.1.1
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
  ncanode-origin:
    image: ozayev/ncanode2.3.0_kc_0.6.1_ca:latest
  ncanode:
    image: images.perfumerlabs.com/dist/ncanode:v3.1.1
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: ncanode
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      NCANODE_REMOTE_URL: http://ncanode-origin:14579
      NCANODE_KEY: /path/to/key
      NCANODE_PWD: 123456
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/ncanode/config) for parameters description.
