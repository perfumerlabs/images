---
layout: default
parent: Request Catcher
title: API
nav_order: 4
---

API Reference
=============

### Send request

`ANY /any/url`

Request example:

`POST http://example.com/path/of/url?a=1&b=2`

Headers:

`X-My-Header: value`

```json
{
    "foo": "bar",
    "baz": 123,
}
```

### Get last received request

`GET /_last_request_`

No request parameters.

Response example (for request mentioned above):

```json
{
    "method": "post",
    "host": "example.com",
    "port": 80,
    "path": "/path/of/url",
    "query": "a=1&b=2",
    "json": {
        "foo": "bar",
        "baz": 123,
    },
    "headers": {
        "x-my-header": "value"
    },
    "body": "{\"foo\":\"bar\",\"baz\":123}",
}
```
