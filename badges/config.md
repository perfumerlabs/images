---
layout: default
parent: Badges
title: Configuration
nav_order: 3
---

Environment variables
=====================

- BADGES_LIFETIME - lifetime of documents. This parameter sets index with "expireAfterSeconds" option. Optional. "2592000" (1 month) by default.
- MG_HOST - mongo server host. Optional. "mongo" by default.
- MG_PORT - mongo server port. Optional. "27017" by default.
- MG_DATABASE - mongo database. Optional. "badges" by default.
- MG_COLLECTIONS - mongo collection names to use separated by a comma. Badges will setup a number of indexes for these collections. Optional.

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.