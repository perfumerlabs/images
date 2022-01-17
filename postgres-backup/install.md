---
layout: default
parent: Postgres Backup
title: Installation
nav_order: 2
---

Installation
============

First, install docker container which makes backups.
We use open source project [prodrigestivill/postgres-backup-local](https://github.com/prodrigestivill/docker-postgres-backup-local).
Generally, it does its work well.

```bash
docker run \
-v /my/backups:/backups
-e POSTGRES_HOST=postgres \
-e POSTGRES_DB=dbname \
-e POSTGRES_USER=user \
-e POSTGRES_PASSWORD=password \
-d prodrigestivill/postgres-backup-local
```

Postgres Backup uses PostgreSQL as database to store information.

You need to install any instance of PostgreSQL server.
It can be either docker container or dedicated installation.
Also it can be your production PostgreSQL instance.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Then, you need to install another instance of PostgreSQL, which will be used to restore dumps to.
DO NOT USE YOUR PRODUCTION INSTANCE OR INSTALL THIS INSTANCE TO YOUR PRODUCTION SERVER.
Restoration of big dumps may consume a lot of resources and lead to overload the server.

Suppose, you installed both PostgreSQL servers and now have PostgreSQL hosts and ports. Then this command installs Postgres Backup.

```bash
docker run \
-p 80:80/tcp \
-e DUMPS_DIR=/my/dumps/dir \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=sms_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e RESTORE_HOST=db \
-e RESTORE_PORT=5432 \
-e RESTORE_USER=user \
-e RESTORE_PASSWORD=password \
-d images.perfumerlabs.com/dist/postgres-backup:v1.0.0
```

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  postgres: # postgresql instance to save statuses about restoration
    image: postgres
    environment:
      POSTGRES_PASSWORD: mysecretpassword
    volumes:
      - /custom/mount:/var/lib/postgresql/data
  restore-postgres: # postgresql instance to restore backups
    image: postgres
    environment:
      POSTGRES_PASSWORD: anotherpassword
  backup: # container which makes backups
    image: prodrigestivill/postgres-backup-local
    environment:
        POSTGRES_HOST=postgres \
        POSTGRES_DB=my_db \
        POSTGRES_USER=postgres \
        POSTGRES_PASSWORD=mysecretpassword \
    volumes:
      - /my/backups/dir:/backups # Backup maker saves dumps to /my/backups/dir on the host
    depends_on:
      postgres:
        condition: service_started
  restore: # container which tests backups
    image: images.perfumerlabs.com/dist/postgres-backup:v1.0.0
    environment:
      PG_HOST: postgres # Links to postgresql instance to save statuses about restoration
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: sms
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      DUMPS_DIR: /opt/backup/dumps # Configure dumps directory in the container (not in the host!)
      RESTORE_HOST: restore-postgres # Links to postgresql instance to restore backups
      RESTORE_PORT: 5432
      RESTORE_USER: postgres
      RESTORE_PASSWORD: anotherpassword
    volumes:
      - /my/backups/dir:/opt/backup/dumps # Mount backups dir /my/backups/dir on the host to our container
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/postgres-backup/config) for parameters description.
