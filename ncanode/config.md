---
layout: default
parent: Ncanode
title: Configuration
nav_order: 3
---

Environment variables
=====================

- PG_HOST - PostgreSQL connection host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required. This is used to pre-create tables on first run.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- NCANODE_REMOTE_URL - full url to original ncanode server. Required.
- NCANODE_KEY - path to private key on the container. Required, if [Ncanode](https://ncanode.kz/) API is planned to be used.
- NCANODE_PWD - password of the private key. Required, if [Ncanode](https://ncanode.kz/) API is planned to be used.
- NCANODE_ENCRYPTION_KEY - when provided, encryption of CMSs is performed before inserting in database. Optional. Generating of the key is done [by this method](https://github.com/defuse/php-encryption/blob/master/docs/classes/Key.md#keycreatenewrandomkey).
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- PHP_MEMORY_LIMIT - memory_limit option in php.ini. Optional. The default value is 128M.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
