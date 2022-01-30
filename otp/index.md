---
layout: default
title: One Time Password
nav_order: 90
has_children: true
---

One time Password
=================

One time Passwords (or OTP) are used in sites to verify, that user owns phone or email they filled in th form.
For that purpose short code is sent to phone number via SMS or to email address via e-mailing and user have to fill in this code in the form,
and, thus, confirm that the phone or email is valid and are owned by him.

This image provides REST API to generate and validate OTPs.
You can manage OTPs via SMS, Email, Telegram or robotic calls.
This image works in conjunction with our other images such as 
[Queue](/images/queue), [SMS](/images/sms) and [Email](/images/email).

Refer to [installation page](/images/otp/install) how to properly set up integrations.

SMS OTP
-------

To create SMS OTP just call:

`POST /sms`

```json
{
    "phone": "77011234567",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

Container internally route request to [SMS](/images/sms) with specified `phone` and `message`.
Also saves `password` and sets it lifetime to specified seconds (5 minutes in the example).

To check password user filled in the form, call:

`GET /sms/check`

```json
{
    "phone": "77011234567",
    "password": "12345"
}
```

Call OTP
--------

Robotic calls uses the same integration as SMS, so request bodies you sent exactly the same as in SMS OTPs,
but endpoints are changed to `POST /call` and `GET /call/check`.

Email OTP
---------

To create Email OTP just call:

`POST /email`

```json
{
    "email": "foobar@example.com",
    "password": "12345",
    "lifetime": 300,
    "subject": "Hello",
    "text": "Your OTP is 12345"
}
```

Container internally route request to [Email](/images/email) with specified `email`, `subject` and `text`.
Also saves `password` and sets it lifetime to specified seconds (5 minutes in the example).

To check password user filled in the form, call:

`GET /email/check`

```json
{
    "email": "foobar@example.com",
    "password": "12345"
}
```

Telegram OTP
------------

To create Telegram OTP call:

`POST /telegram`

```json
{
    "chat_id": "1234567890",
    "password": "12345",
    "lifetime": 300,
    "message": "Your OTP is 12345"
}
```

To send OTP for Telegram this image uses open source project
[1flx/http-telegram-notify](https://github.com/flxs/http-telegram-notify).
Container internally route request to to with specified `chat_id`, `message`.
Also saves `password` and sets it lifetime to specified seconds (5 minutes in the example).

To check password user filled in the form, call:

`GET /telegram/check`

```json
{
    "chat_id": "1234567890",
    "password": "12345"
}
```

Also there is more out-of-the-box functionality. 
Instead of manually call to OTP API you can publish ready-to-use endpoint for the Bot.

1. Configure bot webhook to URL - https://your-otp-host/telegram/webhook. Note that URL must be `https` and internet accessible.
1. When a user types anything to bot, webhook is sent to OTP and it generates new password and send it to bot.
1. Then user can copy the password and fill in the form on your application.
1. Then you can validate this password through `GET /target` endpoint and obtain user's CHAT_ID and save to your storage.
1. Use CHAT_ID then to notify user about something.

Authorization
-------------

If you use telegram otp's and publish OTP service to internet, then some access restriction is needed.
There support of HTTP Basic Authorization.
Fill in `HTTP_AUTH_USERNAME` and `HTTP_AUTH_PASSWORD` variables and authorization at nginx-level will be configured.

Beware, that HTTP Basic Authorization without HTTPS is insecure.
