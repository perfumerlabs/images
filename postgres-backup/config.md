---
layout: default
parent: Postgres Backup
title: Configuration
nav_order: 3
---

Environment variables
=====================

- DUMPS_DIR - directory in the container with dumps (not in the host!). Required.
- PG_HOST - PostgreSQL host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- RESTORE_HOST - PostgreSQL host to restore dumps to. Required. DO NOT USE PRODUCTION POSTGRESQL INSTANCE.
- RESTORE_PORT - PostgreSQL port to restore dumps to. Default value is 5432.
- RESTORE_USER - PostgreSQL user name to restore dumps to. Required.
- RESTORE_PASSWORD - PostgreSQL user password to restore dumps to. Required.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
