---
layout: default
title: NcaLayer Mock
nav_order: 78
has_children: true
---

THIS IMAGE IS ON DEVELOPMENT.
THIS IS JUST ANNOUNCEMENT.

Please, contact us to [support@perfumerlabs.com](mailto:support@perfumerlabs.com) if you want to get this container.
This will encourage us to speed up its development.
We will try to prioritize this task and pack it as soon as possible.

NcaLayer Mock
===============

[NcaLayer](https://pki.gov.kz/en/ncalayer-2/) is a software, which deals with signing data via
Kazakhstan [National Certification Authority](https://pki.gov.kz/en/) certificates.
It is used on thousands of sites across Kazakhstan Internet.

The problem with it is that there is no means to cover sites with auto-tests without complex tricks.
NcaLayer is a desktop application, so testing browser agent just can not interact with it and moreover to choose a certificate and sign document.

This image aims to provide simple websocket server which will interact with browser as NcaLayer.

Usage
-----

- You predefine certificate in container settings.
- Instead of connecting to standard NcaLayer port "wss://127.0.0.1:13579/" you connect in testing environment to NcaLayer Mock wss endpoint.
- When signing event has come, server will sign document with predefined certificate and return public certificate as NcaLayer does.
