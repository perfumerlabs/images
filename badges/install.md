---
layout: default
parent: Badges
title: Installation
nav_order: 2
---

Installation
============

Badges uses Mongo as database to store information.

First, you need to install any instance of Mongo server. It can be either docker container or dedicated installation.
For example, this command installs Mongo from [official docker image](https://hub.docker.com/_/mongo):

```bash
docker run \
-p 27017:27017/tcp \
-v /my/own/datadir:/data/db \
-d mongo
```

Suppose, you installed Mongo server and now have Mongo host and port. Then this command installs Badges.

```bash
docker run \
-p 80:80/tcp \
-e MG_HOST=mongo-host \
-e MG_PORT=27017 \
-e MG_DATABASE=badges \
-e MG_COLLECTIONS=my_collection1,my_collection2 \
-d images.perfumerlabs.com/dist/badge:v2.0.0
```

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  mongo:
    image: mongo
    volumes:
      - /my/own/datadir:/data/db
  badges:
    image: images.perfumerlabs.com/dist/badge:v2.0.0
    environment:
      MG_HOST: mongo-host
      MG_PORT: 27017
      MG_DATABASE: badges
      MG_COLLECTIONS: 'my_collection1,my_collection2'
      TEST: "true"
    depends_on:
      mongo:
        condition: service_started
```

Refer to [configuration page](/images/badges/config) for parameters description.