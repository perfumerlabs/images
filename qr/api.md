---
layout: default
parent: QR generator
title: API
nav_order: 4
---

API Reference
=============

### Create QR from link returning HTML

`GET /qr`

Parameters (query or json):
- data [string,required] - data string to encode to QR code.

Request example:

```html
<a href="http://qr-host.com/qr?data=http://mysite.com/link"></a>
```

or

```json
{
    "data": "http://mysite.com/link"
}
```

Note, that all content provided to query parameter `data` must be url-encoded.