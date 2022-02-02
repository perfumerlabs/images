---
layout: default
parent: Ncanode
title: Configuration
nav_order: 3
---

Environment variables
=====================

- NCANODE_REMOTE_URL - full url to original ncanode server. Required.
- NCANODE_KEY - path to private key on the container. Required, if [Ncanode](https://ncanode.kz/) API is planned to be used.
- NCANODE_PWD - password of the private key. Required, if [Ncanode](https://ncanode.kz/) API is planned to be used.
- NCANODE_ENCRYPTION_KEY - when provided, encryption of CMSs is performed before inserting in database. Optional. Generating of the key is done [by this method](https://github.com/defuse/php-encryption/blob/master/docs/classes/Key.md#keycreatenewrandomkey).

Image requires PostgreSQL instance. See available environment variables [here](/images/software.html#sql-database).

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
