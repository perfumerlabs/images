---
layout: default
parent: ElasticSearch Search-as-you-type
title: Configuration
nav_order: 3
---

Environment variables
=====================

- ES_HOST - Elasticsearch instance url. Required.
- ES_PORT - Elasticsearch instance port. Default is 9200.

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
