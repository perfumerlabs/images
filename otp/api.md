---
layout: default
parent: One Time Password
title: API
nav_order: 4
---

API Reference
=============

### Send sms OTP

`POST /sms`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- phone [string,required] - phone to send to, without "+".
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- message [string,required] - SMS message text to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "phone": "77011234567",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "phone_limit",
    "message": "Phone limit has been reached"
}
```

### Send call OTP

`POST /call`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- phone [string,required] - phone to send to, without "+".
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.

Request example:

```json
{
    "ip": "123.123.123.123",
    "phone": "77011234567",
    "password": "12345",
    "lifetime": 300
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "phone_limit",
    "message": "Phone limit has been reached"
}
```

### Send email OTP

`POST /email`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- email [string,required] - email to send to.
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- subject [string,required] - email subject to send.
- text [string,optional] - email text to send.
- html [string,optional] - email html content to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "email": "foobar@example.com",
    "password": "12345",
    "lifetime": 300,
    "subject": "Hello",
    "text": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "email_limit",
    "message": "Email limit has been reached"
}
```

### Send telegram OTP

`POST /telegram`

Parameters (json):
- ip [string,optional] - IP of requester. Required, if ip limits are set.
- chat_id [string,required] - telegram user CHAT_ID to send to.
- password [string,required] - one time password.
- lifetime [integer,required] - lifetime of password, in seconds.
- message [string,required] - message to send.

Request example:

```json
{
    "ip": "123.123.123.123",
    "chat_id": "1234567890",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

Success response example:

```json
{
    "status": true
}
```

Failure response examples:

```json
{
    "status": false,
    "status_code": "ip_limit",
    "message": "IP limit has been reached"
}
```

```json
{
    "status": false,
    "status_code": "email_limit",
    "message": "Email limit has been reached"
}
```

### Check sms OTP

`GET /sms/check`

Parameters (json):
- phone [string,required] - phone, without "+".
- password [string,required] - one time password.

Request example:

```json
{
    "phone": "77011234567",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Check call OTP

`GET /call/check`

Parameters (json):
- phone [string,required] - phone, without "+".
- password [string,required] - one time password.

Request example:

```json
{
    "phone": "77011234567",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Check email OTP

`GET /email/check`

Parameters (json):
- email [string,required] - email.
- password [string,required] - one time password.

Request example:

```json
{
    "email": "foobar@example.com",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Check telegram OTP

`GET /telegram/check`

Parameters (json):
- chat_id [string,required] - telegram user CHAT_ID.
- password [string,required] - one time password.

Request example:

```json
{
    "chat_id": "1234567890",
    "password": "12345"
}
```

Response example:

```json
{
    "status": true
}
```

### Set SMS limits

`POST /limit/sms`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- phones [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "phones": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

Response example:

```json
{
    "status": true
}
```

### Set CALL limits

`POST /limit/call`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- phones [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "phones": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

Response example:

```json
{
    "status": true
}
```

### Set email limits

`POST /limit/email`

Parameters (json):
- ips [array,optional] - set of rules for IP.
- emails [array,optional] - set of rules for phones.

Each rule consists of minutes and rate parameter which means "how many tries allowed for number of minutes".

Request example:

```json
{
    "ips": [
        {
            "minutes": 10,
            "rate": 3
        },
        {
            "minutes": 60,
            "rate": 10
        }
    ],
    "emails": [
        {
            "minutes": 10,
            "rate": 3
        }
    ]
}
```

### Get last user credentials by a password

`GET /target`

Parameters (json):
- password [string,required] - password string.
- channel [string,required] - `sms`, `email`, `call` or `telegram`

Request example:

```json
{
    "password": "1234567",
    "channel": "telegram"
}
```

Response example:

```json
{
    "status": true,
    "content": {
        "target": "1234567890"
    }
}
```

### Get last valid password by a user credentials

`GET /password`

Parameters (json):
- target [string,required] - phone, email or telegram id.

Request example:

```json
{
    "target": "77011234567"
}
```

Response example:

```json
{
    "status": true,
    "content": {
        "password": "123456"
    }
}
```