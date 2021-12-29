---
layout: default
parent: Email
title: API
nav_order: 4
---

### Send email through SMTP-server

```
POST /smtp
```

Parameters (json):
- subject [string,required] - subject of the message.
- to [array or string,required] - message recipients.
- text [string,optional] - plain text body.
- html [string,optional] - html body.
- signature_enabled [boolean,optional] - whether or not to append signature to the text. Default is "true".

Send HTML email:

```json
{
    "subject": "Hello, world!",
    "to": "bar@example.com",
    "html": "<p>Lorem ipsum dolor sit amet...</p>"
}
```

Send `plain/text` email to multiple email addresses:

```json
{
    "subject": "Hello, world!",
    "to": ["foo@example.com", "bar@example.com", "baz@example.com"],
    "text": "Lorem ipsum dolor sit amet..."
}
```

Disable signature for a particular email:

```json
{
    "subject": "Hello, world!",
    "to": "bar@example.com",
    "html": "<p>Lorem ipsum dolor sit amet...</p>",
    "signature_enabled": false
}
```

Successful response (201 status code):

```json
{
    "status": true
}
```