---
layout: default
title: Ncanode
nav_order: 8
has_children: true
---

What is it
==========

This image deals with Kazakhstan NCA. It provides 3 possibilities:

1. API to store, retrieve, versioning signatures.
1. Convenient API to validate CMS signatures against various situations.
1. Proxy requests to [Ncanode](https://ncanode.kz/) server with built-in private key.

#### Working with signatures

This image does not deal with signatures by itself and requires installation of [Ncanode](https://ncanode.kz/) server.
[Ncanode](https://ncanode.kz/) is needed to validate signatures and sign documents on server-side.

#### Proxy requests

If `POST /origin` request is called, then this request is transparently proxied to [Ncanode](https://ncanode.kz/) API.
One thing this image do, that it set to request private key and password saved in parameters.
The purpose is to hide private key and key password from being revealed.
