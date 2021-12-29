---
layout: default
parent: Email
title: API
nav_order: 4
---

API Reference
=============

### Create a message through SMTP-server

`POST /smtp`

Parameters (json):
- subject [string,required] - subject of the message.
- to [array|string,required] - message recipients.
- text [string,optional] - plain text body.
- html [string,optional] - html body.
- signature_enabled [boolean,optional] - whether or not to append signature to the text. Default is "true".

Request example:
```javascript
{
    "subject": "Hello, world!",
    "to": ["bar@example.com"],
    "html": "<p>Lorem ipsum dolor sit amet...</p>"
}
```

Response example:

```javascript
{
    "status": true
}
```