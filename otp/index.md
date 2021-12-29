---
layout: default
title: OTP
nav_order: 9
has_children: true
---

What is it
==========

This is a container providing one time passwords sending and verifying through sms and email.
This container must be set up in conjunction with [Queue](https://github.com/perfumerlabs/queue), [SMS](https://github.com/perfumerlabs/sms) and [Email](https://github.com/perfumerlabs/email).

#### Steps to configure otp's on your project

1. Configure [Queue](https://github.com/perfumerlabs/queue), [SMS](https://github.com/perfumerlabs/sms) and/or [Email](https://github.com/perfumerlabs/email)
1. Configure this container. Note, that default settings suppose that services above are called `queue`, `sms` and `email` in your cluster.
1. Create OTPs via `POST /sms` or `POST /email`.
1. Check OTPs via `GET /sms/check` or `GET /email/check`

#### Telegram otp's

You can do same setup as above:

1. Create your Telegram Bot.
1. Run [1flx/http-telegram-notify](https://github.com/flxs/http-telegram-notify) docker container.
1. Set OTP_TELEGRAM_URL, OTP_TELEGRAM_APP_TOKEN, OTP_TELEGRAM_WORKER appropriately.
1. Create OTPs via `POST /telegram`.
1. Check OTPs via `GET /telegram/check`.

Also there more out-of-the-box functionality. Instead of manually call to OTP API you can publish ready-to-use endpoint for the Bot.

1. Configure bot webhook to URL - https://your-otp-host/telegram/webhook. Note that URL must be `https` and internet accessible.
1. When a user types anything to bot, webhook is sent to OTP and it generates new password and send it to bot.
1. Then user can copy the password and fill in the form on your application.
1. Then you can validate this password through `GET /target` endpoint and obtain user's CHAT_ID and save to your storage.
1. Use CHAT_ID then to notify user about something.

#### Authorization

If you use telegram otp's and publish OTP service to internet, then some access restriction is needed.
There support of HTTP Basic Authorization.
Fill in `HTTP_AUTH_USERNAME` and `HTTP_AUTH_PASSWORD` variables and authorization at nginx-level will be configured.

Beware, that HTTP Basic Authorization without HTTPS is insecure.
