---
layout: default
parent: Feed
title: Installation
nav_order: 2
---

Installation
============

Feed uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs Feed.

```bash
docker run \
-e PG_REAL_HOST=db \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=feed_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/feed:v2.0.0
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
  feed:
    image: images.perfumerlabs.com/dist/feed:v2.0.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: feed
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/feed/config) for parameters description.
