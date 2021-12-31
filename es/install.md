---
layout: default
parent: ElasticSearch Fulltext
title: Installation
nav_order: 2
---

Installation
============

Fulltext uses ElasticSearch as database to store information.

First, you need to install any instance of ElasticSearch server. It can be either docker container or dedicated installation.
For example, this command installs ElasticSearch from [official docker image](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html):

```bash
docker run \
-p 9200:9200/tcp \
-v esdata:/usr/share/elasticsearch/data \
-e "discovery.type=single-node"
-d docker.elastic.co/elasticsearch/elasticsearch:7.16.2
```

Suppose, you installed ElasticSearch server and now have host and port. Then this command installs Fulltext.

```bash
docker run \
-p 80:80/tcp \
-e ES_HOST=elasticsearch \
-e ES_PORT=9200 \
-d images.perfumerlabs.com/dist/es:v1.1.2
```

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.16.2
    environment:
      - discovery.type=single-node
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
  fulltext:
    image: images.perfumerlabs.com/dist/es:v1.1.2
    environment:
      ES_HOST: elasticsearch
      ES_PORT: 9200
    depends_on:
      elasticsearch:
        condition: service_started
```

Refer to [configuration page](/images/es/config) for parameters description.