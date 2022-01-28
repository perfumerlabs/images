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
- provider [string,optional] - provider code, "smscru", "twilio", "epochtasmsru", "mobizonkz" values are acceptable. If not set, SMS_PROVIDER environment will be used.

Request example:

```json
{
    "provider": "twilio",
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

### Call

`POST /call`

Parameters (json):
- phones [array or string,required] - phones to send to, without "+".
- message [string,required] - message text to send.
- force [bool,optional] - if "true" ignores blacklisting. Default is false.
- provider [string,optional] - provider code, "smscru" values are acceptable. If not set, SMS_PROVIDER environment will be used.

Request example:

```json
{
    "provider": "smscru",
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

`DELETE /blacklist`

Parameters (json):
- phone [string,required] - phone to delete, without "+".

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

### Check if phone in blacklist

`GET /blacklist`

Parameters (json):
- phone [string,required] - phone to check, without "+".

Request example:

```json
{
    "phone": "77011234567"
}
```

If phone is not in the blacklist, `400 Bad Request` status code will be returned.

Response example, if phone in the blacklist (`200 Ok` status code):

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
