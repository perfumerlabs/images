---
layout: default
title: Software
nav_order: 2
---

This chapter consists of information about what software we use to handle functionality.

Programming language
--------------------

All our images at the moment are written on PHP8.
PHP8 is fast, modern and reliable language.

Operational system
------------------

All images built from latest "ubuntu:focal" base image.
We carefully watch for security updates and rebuild images as soon as possible.

Incoming traffic
----------------

All HTTP traffic to our containers must go to 80 port,
until different is mentioned in documentation.

Every image is equipped with nginx to get traffic and route it to fastcgi upstream.
We know, that many developers prefer to spread nginx and fastcgi servers.
We specially bundle all software into 1 container to make it simpler to deploy our images.
You deploy only 1 container instead of 2 (or sometimes 3) containers. 
Also in several images we have customized nginx config files.
We believe, that our customer should not think about how nginx should be deployed.

SSL
---

None of our containers supports SSL traffic, until different is mentioned in documentation.

In Kubernetes cluster there is special service loadbalancer to terminate SSL traffic for these purposes.
It can be Ingress or HaProxy.

SQL Database
------------

Many of our images need SQL database to store information.
We use PostgreSQL for these purposes.
We don't bundle none of our images with PostgreSQL server.
You should have it on your side.

Every image, which deals with PostgreSQL, supports several environment parameters:

- `PG_REAL_HOST` - real hostname of master server instance (not PgBouncer or other connection pool). It is used to create database or scheme and make migrations.
- `PG_HOST` - hostname of master server to connect regular traffic to (it can be PgBouncer or other connection pool). 
- `PG_PORT` - port og instance. Default is 5432.
- `PG_SLAVES` - hostnames of slave instances separated by a comma, for example, `slave1,slave2`
- `PG_USER` - database user software will use to connect to instances with.
- `PG_PASSWORD` - database user password software will use to connect to instances with.
- `PG_DATABASE` - database to store information, if it is missing, it will be created
- `PG_SCHEMA` - database schema to store information, if it is missing, it will be created. Default to `public`.

All of our images dealing with PostgreSQL has their own unique table prefix.
For example, SMS image creates tables with `sms_*` naming convention
or OTP image creates tables with `otp_*` naming convention.

There will be no problem if you configure images to same database and `public` scheme.
But the best practice we suppose is to have 1 database and to use different schema for different container.

NoSQL databases
---------------

Several of our images uses NoSQL databases to store information.
As a rule, we don't bundle Database instance to container, until different is mentioned in documentation.
So you must to configure your instance.

Testing
-------

We cover 99% functionality of our images with automated functional tests.
We use open-source [Codeception](https://codeception.com) library to write tests.