---
layout: default
parent: Queue
title: Fraction task
nav_order: 5
---

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