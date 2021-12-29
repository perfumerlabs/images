---
layout: default
parent: SMS
title: API
nav_order: 4
---

API Reference
=============

### Send sms

`POST /sms`

Parameters (json):
- phones [array or string,required] - phones to send to, without "+".
- message [string,required] - message text to send.
- force [bool,optional] - if "true" ignores blacklisting. Default is false.
- type [string,optional] - "sms" or "call". Default is "sms".

Request example:

```json
{
    "phones": ["77011234567", "77070001234"],
    "message": "Hello, world!"
}
```

Response example:

```json
{
    "status": true
}
```

### Add phone to blacklist

`POST /blacklist`

Parameters (json):
- phone [string,required] - phone to add, without "+".

Request example:

```json
{
    "phone": "77011234567"
}
```

Response example:

```json
{
    "status": true
}
```

### Delete phone from blacklist

`DELETE /blacklist/{:phone}`

Parameters (url):
- phone [string,required] - phone to delete, without "+".

Request example:

```query
DELETE /blacklist/77011234567
```

Response has 404 status code, if not in the blacklist.

Response example, if in the blacklist:

```json
{
    "status": true
}
```

### Check if phone in blacklist

`GET /blacklist/{:phone}`

Parameters (url):
- phone [string,required] - phone to check, without "+".

Request example:

```query
GET /blacklist/77011234567
```

Response has 404 status code, if not in the blacklist.

Response example, if in the blacklist:

```json
{
    "status": true,
    "content": {
        "blacklist": {
            "phone": "77011234567",
            "created_at": "2020-09-01 00:00:00"
        }
    }
}
```