---
layout: default
parent: LDAP authentication
title: Installation
nav_order: 2
---

Installation
============

LDAP does not have any dependencies.
It can be installed via following command:

```bash
docker run \
-p 80:80/tcp \
-e LDAP_HOST=my-ldap.com \
-e LDAP_PORT=389 \
-e LDAP_BIND_DN="uid={{ "{{username" }}}},dc=example,dc=com" \
-e LDAP_ENCRYPTION=ssl \
-d images.perfumerlabs.com/dist/ldap:v1.0.0
```

Refer to [configuration page](/images/ldap/config) for parameters description.
