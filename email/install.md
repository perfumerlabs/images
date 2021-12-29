---
layout: default
parent: Email
title: Installation
nav_order: 2
---

Installation
============

Suppose, we want to set up this container to send emails through Google SMTP.
From [documentation](https://developers.google.com/gmail/imap/imap-smtp) we know settings such as host, port or encryption method.
Next command runs container with those parameters:

```bash
docker run \
-p 80:80/tcp \
-e EMAIL_FROM=your-gmail-account@gmail.com \
-e SMTP_HOST=smtp.gmail.com \
-e SMTP_PORT=465 \
-e SMTP_USERNAME=your-gmail-account@gmail.com \
-e SMTP_PASSWORD=your-gmail-password \
-e SMTP_ENCRYPTION=tls \
-d images.perfumerlabs.com/dist/email:v1.2.1
```

Queueing
--------

Lets add queueing to our service. We use our ready-to-use [Queue](/images/queue) container.
With next command we create Queue container with 1 worker sending emails (you can set any number).

```bash
docker run \
-p 80:80/tcp \
-e "QUEUE_WORKERS={\"email\":1}" \
-v tarantool:/var/lib/tarantool \
-d images.perfumerlabs.com/dist/queue:v1.4.1
```

Tie all together with Docker Compose:

```yml
version: '2.2'
services:
  queue:
    image: images.perfumerlabs.com/dist/queue:v1.4.1
    environment:
      QUEUE_WORKERS: "{\"email\":1}"
    volumes:
      - tarantool:/var/lib/tarantool
  email:
    image: images.perfumerlabs.com/dist/otp:v2.4.1
    environment:
      EMAIL_FROM: your-gmail-account@gmail.com
      SMTP_HOST: smtp.gmail.com
      SMTP_PORT: 465
      SMTP_USERNAME: your-gmail-account@gmail.com
      SMTP_PASSWORD: your-gmail-password
      SMTP_ENCRYPTION: tls
```

Now, instead of sending emails directly to Email container we send request to Queue container.
And it then requests Email container in the background by itself.
Typical request can be:

```
POST http://queue/task
```

```json
{
    "worker": "email",
    "url": "http://email/smtp",
    "method": "post",
    "json": {
        "to": "recipient@example.com",
        "subject": "Hi",
        "html": "<p>Hello, World!</p>"
    }
}
```

Refer to [Queue](/images/queue) for detailed options.