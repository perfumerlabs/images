---
layout: default
title: Feed
nav_order: 7
has_children: true
---

What is it
==========

Docker container for building personal feeds or chats.

How it works
============

This microservice is invented to provide data structure and API for personal feeds like notification feed,
some kind of article feed with personal algorithms, chats and so on.

When a record appears you push it to Feed. Then you can fetch a number of records
from the top of list or fetch a particular record and send it to client-side.
Feed has integration with websocket service [Centrifugo](https://github.com/centrifugal/centrifugo) and [Badges](https://github.com/perfumerlabs/badges) microservice.
If config for those integrations is set, then Feed will send pushes to the service by itself.
Also Feed will automatically delete badges from [Badges](https://github.com/perfumerlabs/badges) when read API is called.
