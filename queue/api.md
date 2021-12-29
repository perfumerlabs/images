---
layout: default
parent: Queue
title: API
nav_order: 4
---

API Reference
=============

### Create new regular task

`POST /task`

- worker: which tube will execute task. Required.
- url: URL that will be called. Required.
- method: HTTP request method that will be called. Required.
- delay: delay in seconds. Required, if no "datetime" provided.
- datetime: Datetime of execution. Format "YYYY-MM-DD HH:ii:ss". UTC. Required, if no "delay" provided.
- headers: headers that will be sent with the request. Optional.
- json: json body that will be sent with the request. Optional.
- query_string: object will be transformed to query string (for example, "param1=foo&param2=bar") and sent as body. If request method is GET or HEAD it will be appended to url. Optional.
- body: Raw body that will be sent with the request. Optional.
- sleep: Time in microseconds to sleep after execution of the task. Optional.
- timeout: Time in seconds to change timeout, when queue worker requests backend. Optional. If not set, `QUEUE_DEFAULT_TIMEOUT` is applied.

Use one of "delay" or "datetime". Use one of "json", "query_string" or "body".

Request body example:

```json
{
    "worker": "foo",
    "url": "http://my-site.com/action1",
    "method": "post",
    "delay": 30,
    "headers": {
        "Authorization": "Bearer my_token"
    },
    "json": {
        "param": "value"
    }
}
```

Request body example:

```json
{
    "worker": "bar",
    "url": "http://my-site.com/action2",
    "method": "get",
    "datetime": "2019-01-01 10:00:00",
    "query_string": {
        "param": "value"
    }
}
```

Request body example:

```json
{
    "worker": "baz",
    "url": "http://my-site.com/action3",
    "method": "put",
    "delay": 30,
    "body": "raw body",
    "sleep": 100
}
```

Response example:

```json
{
    "status": true
}
```

### Create new fraction task

`POST /fraction`

- Same parameters as in regular task creation.
- min [integer]: lower bound of gap
- max [integer]: upper bound of gap
- gap [integer]: size of gap. Minimum allowed value is 10.

Request body example:

```json
{
    "worker": "foo",
    "url": "http://my-site.com/action1",
    "method": "post",
    "delay": 30,
    "headers": {
        "Authorization": "Bearer my_token"
    },
    "json": {
        "param": "value"
    },
    "min": 1000,
    "max": 1000000,
    "gap": 100
}
```

Response example:

```json
{
    "status": true
}
```