---
layout: default
title: About
nav_order: 1
---

Images documentation
====================

Hello, my name is Ilyas Makashev. This is documentation in Jekyll format for all Docker images I develop.

Nowadays, every development consists of working with different external services, storages, data structures, etc,
making it hard to start any project from scratch.
That's why at some moment I decided to wire up different functionality into reusable versionable Docker images instead of copying source again and again.

I aim to provide as much as possible ready-to-use Docker images to cover different side-logic functionality.
I try to convert complex integrations and data storage syntax and logic into simple REST API server,
which is familiar to even inexperienced developers.

I don't provide any Site-as-a-Service-like services or any other online services.
All software is on-premise Docker images, which can be installed to private servers.

License
-------

All software is distributed under closed license.
But I am happy to share code and images on free basis.
Just let me know to the [email](mailto:mehmatovec@gmail.com), [github account](https://github.com/blumfontein) or [LinkedIn](https://www.linkedin.com/in/ilyas-makashev-32410a71/), and I provide you access to my GitLab instance with the sources.

Below is a categorized list of currently available Docker images.
I actively add new software to my collection and release new versions, when the need appears in my jobs.

Files and Images
----------------

1. [HTML to PDF converter](https://perfumerlabs.github.io/images/pdf/) based on LibreOffice tool.
1. [QR](https://perfumerlabs.github.io/images/qr/) is a simple QR-code generator from URL.
1. [Upload Server](https://perfumerlabs.github.io/images/upload/) provides API to store images, files, video and audio, as well as actions such as cropping, resizing, rotating and compression.
1. [Charts](https://perfumerlabs.github.io/images/charts/) provides fast implementation of some kind of charts through URL.

Different Storage Integrations
------------------------------

1. [Delivery](https://perfumerlabs.github.io/images/delivery/) is an engine to automate custom deliveries with built-in integration with our other services such as Email or SMS.
1. [ElasticSearch Fulltext search](https://perfumerlabs.github.io/images/es-fulltext/) is simplified API (without no need to learn ElasticSearch syntax) to create, index and search through ElasticSearch collections built with Fulltext search template.
1. [ElasticSearch Search-as-you-type](https://perfumerlabs.github.io/images/es-sayt/) is simplified API (without no need to learn ElasticSearch syntax) to create, index and search through ElasticSearch collections built with Search-as-you-Type search template.
1. [One Time Password](https://perfumerlabs.github.io/images/otp/) generator provides storing and checking one time passwords, with built-in integration with our services such as Email and SMS.
1. [Notifications Center](https://perfumerlabs.github.io/images/notify/) is a place to manage notifications content and which channels (SMS, Email) to send it through.
1. [Queue Server](https://perfumerlabs.github.io/images/queue/) is a bundled simplified server to perform queuing operations.

Payment Gateways Integrations
-----------------------------

1. [CloudPayments payments](https://perfumerlabs.github.io/images/cloudpayments/)
1. [Kassa24 payments](https://perfumerlabs.github.io/images/kassa24/)
1. [Paybox payments](https://perfumerlabs.github.io/images/paybox/)
1. [Qiwi payments](https://perfumerlabs.github.io/images/qiwi/)
1. [Wooppay payments](https://perfumerlabs.github.io/images/wooppay/)

Other Services Integrations
----------------------------

1. [Telegram Bridge](https://perfumerlabs.github.io/images/telegram-bridge/) is a middleware container to communicate with Telegram Bots: saving history, chat ids, chat states, etc.
1. [Email delivery](https://perfumerlabs.github.io/images/email/) is a RESTful API endpoint to send emails through SMTP gateway.
1. [LDAP](https://perfumerlabs.github.io/images/ldap/) is RESTful API endpoint to authenticate against LDAP server (i.e. ActiveDirectory) without no need to learn LDAP terms such as bindings and distinguished names.
1. [SMS](https://perfumerlabs.github.io/images/sms/) is an integration with different SMS and robotic calls gateways on the market.

Data Structures Images
----------------------

1. [Badges](https://perfumerlabs.github.io/images/badges/) is a storage to provide REST API to store, retrieve and delete countable events.
1. [Box](https://perfumerlabs.github.io/images/box/) is a simplified intermediate storage to exchange data between multiple projects.
1. [Feed](https://perfumerlabs.github.io/images/feed/) is a data structure in PostgreSQL database, which provides REST API to store, retrieve and delete records for any feed-like functionality on the site.
1. [Questions-Answers](https://perfumerlabs.github.io/images/questions/) is our custom YAML-based protocol to describe questions tree to build functionality such as quizes and questionnaires in the site.

Testing Tools
-------------

1. [PostgreSQL Backups Tester](https://perfumerlabs.github.io/images/postgres-backup/) is small service, which continuously tests your PostgreSQL backups for corruption.
1. [Request Catcher](https://perfumerlabs.github.io/images/request-catcher/) is a tool for catching web requests for testing webhooks, http clients and other applications that communicate over http, especially helpful to cover it with autotests.
1. [NcaLayer Mock](https://perfumerlabs.github.io/images/ncalayer-mock/) is a websocket server which acts just like [NcaLayer](https://pki.gov.kz/en/ncalayer-2/) making it possible to cover web-site with autotests such as Selenium or Katalon.

Kazakhstan NCA certificates
---------------------------

1. [NCA Signatures](https://perfumerlabs.github.io/images/ncanode/) - is a data structure in PostgreSQL database for Kazakhstan NCA certificates signed documents, also it provides convenient API endpoints to verify signatures data.
