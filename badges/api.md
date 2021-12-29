---
layout: default
parent: Badges
title: API
nav_order: 4
---

API Reference
=============

### Create a badge

`POST /badge`

Parameters (json):
- collection [string,required] - Name of the mongo collection.
- name [string,required] - Name of the badge.
- user [string,required] - Name of the user.
- payload [string,optional] - optional extra data.

Request example:
```javascript
{
    "collection": "my_collection1",
    "name": "profile/1",
    "user": "1",
    "payload": {
        "foo": "bar"
    }
}
```

Response example:

```javascript
{
    "status": true
}
```

### Get badges

`GET /badges`

Parameters (json):
- collection [string,required] - Name of the mongo collection.
- user [string,required] - Name of the user.
- name [string,optional] - Name of the badge.

Request example:
```javascript
{
    "collection": "my_collection1",
    "name": "profile/1",
    "user": "1"
}
```

Response example:

```javascript
{
    "status": true,
    "content": {
        "badges": [
            {
                "name": "profile/1",
                "user": "1",
                "payload": {
                    "foo": "bar"
                },
                "created_at": "2020-06-20 08:43:16"
            }
        ]
    }
}
```

### Delete badges

`DELETE /badges`

Parameters (json):
- collection [string,required] - Name of the mongo collection.
- user [string,required] - Name of the user.
- name [string,optional] - Name of the badge.

Request example:
```javascript
{
    "collection": "my_collection1",
    "name": "profile/1",
    "user": "1"
}
```

Response example:

```javascript
{
    "status": true
}
```

### Get badge counters

`GET /counters`

Parameters (json):
- collection [string,required] - Name of the mongo collection.
- names [string or array,required] - Names of badges, array of names or list of names separated by a comma. If you want to count all badges, provide "_all" key.
- user [string,required] - Name of the user.

Request example:
```javascript
{
    "collection": "my_collection1",
    "names": ["profile/1", "profile/2", "_all"],
    "user": "1"
}
```

Response example:

```javascript
{
    "status": true,
    "content": {
        "counters": {
            "profile/1": 1,
            "profile/2": 2,
            "_all": 3
        }
    }
}
```
