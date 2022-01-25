---
layout: default
title: LDAP authentication
nav_order: 77
has_children: true
---

What is it
==========

This is container providing convenient API to communicate with LDAP server.
Currently only "login" action is implemented.

Usage
-----

Consider, you have configured LDAP server at "example.com" host and 389 port.
Say, your LDAP catalogue consists of accounts with following distinguished names:
"uid=ben,dc=example,dc=com", "uid=robert,dc=example,dc=com", "uid=alice,dc=example,dc=com" and so on.

Your users know their logins (ben, robert, alice) and use them to authenticate in different services.
And you want to set up LDAP authentication in some web-resource.

Then, first, install the container:

```bash
docker run \
-e LDAP_HOST=example.com \
-e LDAP_PORT=389 \
-e LDAP_BIND_DN="uid={{username}},dc=example,dc=com" \
-d images.perfumerlabs.com/dist/ldap:v1.0.0
```

You specify `LDAP_HOST`, `LDAP_PORT` and `LDAP_BIND_DN`.
Note, how we set `LDAP_BIND_DN` parameter: uid={{username}},dc=example,dc=com.
"{{username}}" is a special substitution, which is used to construct proper user distinguished name in LDAP catalogue.

To log in against LDAP server send next request to container:

`POST /login`

```json
{
    "username": "ben",
    "password": "bens_password"
}
```

When request sent, the container sets received username (ben) and replaces "{{username}}" in configured `LDAP_BIND_DN`.
Thus proper distinguished name is built ("uid=ben,dc=example,dc=com" in example).

If username and password parameters are valid, then json object with account attributes is returned (refer to [API page](/images/ldap/api)).
If not, status code `401 Unauthorized` is returned.

LDAPS
-----

If LDAP server is configured through SSL certificates, provide `LDAP_ENCRYPTION=ssl` during container setup.