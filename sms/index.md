---
layout: default
title: SMS
nav_order: 110
has_children: true
---

What is it
==========

This is a container providing API to send sms through external SMS providers.
Currently only [SMSC.RU](https://smsc.ru/) provider is supported.
It is recommended to set up this container to work in conjunction with [Queue](https://github.com/perfumerlabs/queue) container.

Supported providers for now
===========================

1. [smsc.ru](https://smsc.ru) - code "smscru"

Feel free to request support for any other sms providers.
