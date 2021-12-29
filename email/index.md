---
layout: default
title: Email
nav_order: 5
has_children: true
---

What is it
==========

Sending emails is often operation in any project.
[SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) - is the popular protocol to send emails.
There plenty of free and paid providers of SMTP servers.

This container provides API to send emails through external SMTP server.
Instead of integrating libraries to your project you can just call regular HTTP API instead.
Quick example:

```
POST /smtp

{
    "to": "recipient@example.com",
    "subject": "Hi",
    "html": "<p>Hello, World!</p>"
}
```

Queueing
--------

Sending emails without using any queueing is generally not the best practice.
If you send too much emails in a period of time you can reach your provider limits or overload your email sending server.

We recommend to use our ready-to-use [Queue](/images/queue) image.

Read more about installation in [Installation](/images/email/install) chapter.

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

Refer to [configuration](/images/email/config) page.