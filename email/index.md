---
layout: default
title: Email
nav_order: 50
has_children: true
---

Email sending through SMTP
==========================

This image provides very simple way to connect your backend to email deliveries.

Just install container, call very simple REST API endpoint and you are done, without no need to explore email libraries in your favourite language.

Usage
-----

Container works only with providers of [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) mailing services.
There plenty of free and paid providers of SMTP servers.

For example, you can use very popular [SendPulse](https://sendpulse.com/), they provide till 10 thousand free emails per month.
If you register, then on [SMTP settings page](https://login.sendpulse.com/smtp/index/settings/) you can find your SMTP credentials.
Installation of container can be done with this command:

```bash
docker run \
-e EMAIL_FROM=noreply@your-domain.com \
-e SMTP_HOST=smtp-pulse.com \
-e SMTP_PORT=465 \
-e SMTP_USERNAME=sendpulse-account \
-e SMTP_PASSWORD=sendpulse-password \
-e SMTP_ENCRYPTION=ssl \
-d images.perfumerlabs.com/dist/email:v1.3.0
```

After setup call this API in the code to send email:

```
POST http://email-container/smtp

{
    "to": "recipient@example.com",
    "subject": "Hi",
    "text": "Hello, World"
}
```

or `html` content:

```
POST http://email-container/smtp

{
    "to": "recipient@example.com",
    "subject": "Hi",
    "html": "<p>Hello, World!</p>"
}
```

Signatures
----------

Often it is needed to append all outgoing emails with some permanent text, called signature.
Instead of dealing with it in your application, you can set up it in the container.

Provide `EMAIL_SIGNATURE_HTML` and `EMAIL_SIGNATURE_TEXT` parameters to container setup. For example:

```bash
docker run \
...
-e EMAIL_SIGNATURE_TEXT="Best regards, your Bob" \
-e EMAIL_SIGNATURE_HTML="<p>Best regards, your Bob</p>" \
...
```

And then all emails body will be appended with specified content.

Refer to [configuration](/images/email/config) page.

Queueing
--------

Sending emails without using any queueing is generally not the best practice.
If you send too much emails in a period of time you can reach your provider limits or overload your email sending server.

We recommend to use our ready-to-use [Queue](/images/queue) image.

Read more about installation in [Installation](/images/email/install) chapter.
