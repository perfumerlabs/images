---
layout: default
title: SMS Gateways
nav_order: 110
has_children: true
---

SMS Gateways
============

This is an image providing API to send sms and robotic calls through several external SMS providers.
You don't need to learn API docs in particular providers site.
All providers API is simplified and unified into single API.

Providers
---------

Currently, we support 4 SMS Providers

1. [Twilio](https://twilio.com) - big international provider
1. [SMSC](https://smsc.ru) - biggest Russia Federation Provider
1. [Epochta](https://www.epochta.ru/) - another Russia Federation provider
1. [Mobizon](https://mobizon.kz) - Kazakhstan provider

Usage
-----

For example, you have account in [SMSC](https://smsc.ru) service.
Then, install container with this command (SMS uses PostgreSQL to persist some information, so you need to install an instance of it first, refer to [installation docs](/images/sms/install)):

```bash
docker run \
-e SMS_PROVIDER=smscru \
-e SMSCRU_SENDER=your_smscru_sender_name \
-e SMSCRU_USERNAME=your_smscru_username \
-e SMSCRU_PASSWORD=your_smscru_password \
-e PG_HOST=db \
-e PG_REAL_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=sms_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-d images.perfumerlabs.com/dist/sms:v2.1.0
```

After setup call this API in your code to send SMS:

```
POST http://sms-container/sms

{
    "phones": "71234567890",
    "message": "Hi"
}
```

And that's it. You can see some logging output in the container STDOUT.

Calls
-----

Another often used feature is calling to user with robot and say something, for example, for example numeric code for login (one time password).
Almost every SMS provider provides this functionality too.

To send `call` request make this API request in your code:

```
POST http://sms-container/call

{
    "phones": "71234567890",
    "message": "Hi"
}
```

And this will work.

Note, that for the moment, only SMSC.ru "call" integration is accessible.

Blacklisting
------------

Container also supports setting phones to blacklist.
Blacklisted phones don't receive sms or calls.
To blacklist a phone make request:

```
POST http://sms-container/blacklist

{
    "phone": "71234567890"
}
```

Check out more options at [API](/images/sms/api) page.

Queueing
--------

Sending SMS without using any queueing is generally not the best practice.
If you send too much sms in a period of time you can reach your provider limits.

We recommend to use our ready-to-use [Queue](/images/queue) image.

Read more about installation in [Installation](/images/sms/install) chapter.
