---
layout: default
title: Software
nav_order: 2
---

This chapter consists of information about what software we use in development.

Programming language
--------------------

All our images at the moment are written on PHP8.
PHP8 is fast, modern and reliable language.

Operational system
------------------

All images built from latest "ubuntu:focal" base image.
We carefully watch for security updates and rebuild images as soon as possible.

SQL Database
------------

Many of our images need SQL database to store information.
We use PostgreSQL for these purposes.
We don't bundle none of our images with PostgreSQL server.
You should have it on your side.

Every image, which deals with PostgreSQL, supports several environment parameters:

- `PG_REAL_HOST` - real hostname of master server instance (not PgBouncer or other connection pool). It is used to create database or schema and make migrations.
- `PG_HOST` - hostname of master server to connect regular traffic to (it can be PgBouncer or other connection pool). 
- `PG_PORT` - port of instance. Default is 5432.
- `PG_SLAVES` - hostnames of slave instances separated by a comma, for example, `slave1,slave2`
- `PG_USER` - database user software will use to connect to instances with.
- `PG_PASSWORD` - database user password software will use to connect to instances with.
- `PG_DATABASE` - database to store information, if it is missing, it will be created
- `PG_SCHEMA` - database schema to store information, if it is missing, it will be created. Default to `public`.

All of our images dealing with PostgreSQL has their own unique table prefix.
For example, SMS image creates tables with `sms_*` naming convention
or OTP image creates tables with `otp_*` naming convention.

There will be no problem if you configure images to same database and `public` schema.
But the best practice we suppose is to have 1 database and to use different schema for different container.

PHP configuration
-----------------

As a rule we use regular PHP-FPM server to handle requests.
All images have certain environment variables to configure PHP performance.

PHP has a worker - a unit which handles a request.
One worker can handle 1 request at a time.
So more workers can handle more simultaneous requests, but require more RAM.
All images are configured to have `pm = static` in `www.conf` file ([PHP docs](https://www.php.net/manual/en/install.fpm.configuration.php)).
Below a list of parameters to customize PHP options.

- `PHP_PM_MAX_CHILDREN` - number of FPM workers, parameter `pm.max_children` in `www.conf` file. Default value is 30. This is a main option, consider to increase it, if you face strange errors in STDOUT of container.
- `PHP_PM_MAX_REQUESTS` - number of FPM max requests, parameter `pm.max_requests` in `www.conf` file. Default value is 300.
- `PHP_MAX_EXECUTION_TIME` - maximum time in seconds a request can live, parameter `max_execution_time` in `php.ini` file. Default value can be various in particular images, but as a rule it is 30.
- `PHP_MEMORY_LIMIT` - maximum amount of RAM a request can consume, parameter `memory_limit` in `php.ini` file. Default value can be various in particular images, but as a rule it is 128M.

NoSQL databases
---------------

Several of our images uses NoSQL databases to store information.
As a rule, we don't bundle Database instance to container, until different is mentioned in documentation.
So you must to configure your instance.

Incoming traffic
----------------

All HTTP traffic to our containers must go to 80 port,
until different is mentioned in documentation.

Every image is equipped with nginx to get traffic and route it to fastcgi upstream.
We know, that many developers prefer to spread nginx and fastcgi servers.
We specially bundle all software into 1 container to make it simpler to deploy our images.
You deploy only 1 container instead of 2 (or sometimes 3) containers.
Also in several images we have customized nginx config files.

Authentication
--------------

Most of images don't have any authentication mechanisms, until different is mentioned in the documentation.
Because they are not supposed to be internet accessible.
So, for security reasons pay attention not to expose any container ports to world-wide.

SSL
---

None of our containers supports SSL traffic, until different is mentioned in documentation.

In Kubernetes cluster there is special service loadbalancer to terminate SSL traffic for these purposes.
It can be Ingress or HaProxy.

Testing
-------

We cover 99% functionality of our images with automated functional tests.
We use open-source [Codeception](https://codeception.com) library to write tests.
