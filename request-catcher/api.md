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
    "query": {
        "a": 1,
        "b": 2,
    },
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

Response parameters:

- method[string] - Request method of catched request.
- host[string] - Request hostname of catched request.
- port[integed] - Request port number of catched request.
- path[string] - URL path part of catched request.
- query[object] - query string of catched request in form of object for convenience in autotests, because order of query string parameters can change unexpectedly.
- json[object] - is a json body string of catched request, if it is not json request, then it will be "null".
- headers[object] - headers object of catched request.
- body[string] - raw body of catched request.

Response status codes:

- If content of last request exists, then `200 Ok` status code and json like above will be returned.
- If content is missing then `204 No content` status code is returned.

### Delete last received request

`DELETE /_last_request_`

No request parameters.