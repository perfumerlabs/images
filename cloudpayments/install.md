---
layout: default
parent: Cloudpayments
title: Installation
nav_order: 2
---

Installation
============

Cloudpayments Payments uses PostgreSQL as database to store information.

First, you need to install any instance of PostgreSQL server. It can be either docker container or dedicated installation.
For example, this command installs PostgreSQL from [official docker image](https://hub.docker.com/_/postgres):

```bash
docker run \
-v /custom/mount:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=mysecretpassword
-d postgres
```

Suppose, you installed PostgreSQL server and now have PostgreSQL host and port. Then this command installs Cloudpayments Payments.

```bash
docker run \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=cloudpayments \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e CLOUDPAYMENTS_PUBLIC_ID=123456 \
-e CLOUDPAYMENTS_CHECK_URL=http://example.com/my-check-url \
-e CLOUDPAYMENTS_WEBHOOK_URL=http://example.com/my-webhook-url \
-e HTTP_AUTH_USERNAME=http_username \
-e HTTP_AUTH_PASSWORD=http_password \
-d images.perfumerlabs.com/dist/cloudpayments:v1.0.0
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
  cloudpayments:
    image: images.perfumerlabs.com/cloudpayments:v1.0.0
    environment:
      PG_HOST: postgres
      PG_REAL_HOST: postgres
      PG_PORT: 5432
      PG_DATABASE: cloudpayments
      PG_USER: postgres
      PG_PASSWORD: mysecretpassword
      CLOUDPAYMENTS_PUBLIC_ID: 123456
      CLOUDPAYMENTS_CHECK_URL: http://example.com/my-check-url
      CLOUDPAYMENTS_WEBHOOK_URL: http://example.com/my-webhook-url
      HTTP_AUTH_USERNAME: http_username
      HTTP_AUTH_PASSWORD: http_password
    depends_on:
      postgres:
        condition: service_started
```

Refer to [configuration page](/images/cloudpayments/config) for parameters description.
