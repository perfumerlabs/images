---
layout: default
parent: LDAP authentication
title: API
nav_order: 4
---

API Reference
=============

### Check credentials against LDAP server

`POST /login`

Request parameters (json):
- username [string,required] - username of LDAP account.
- password [string,required] - password of LDAP account.
- attributes [array,optional] - optional array of LDAP account attributes to return to. Default list is: cn, sn, givenName, mail, ou, mobile, uid.

Request example:

```json
{
    "username": "my_username",
    "password": "my_password",
    "attributes": ["cn", "sn", "givenName"]
}
```

Response parameters (json):
- entry [object] - found account object wuth requested attributes

Response example:

```json
{
    "status": true,
    "content": {
        "entry": {
            "cn": "Bender Bending Rodríguez",
            "sn": "Rodríguez",
            "givenName": "Bender"
        }
    }
}
```

Response errors:

- `400 Status Code`: username not set
- `401 Status Code`: credentials invalid
- `503 Status Code`: connection to LDAP server can't be established
- `504 Status Code`: connection to LDAP server timed out