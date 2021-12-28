---
layout: default
title: Badges
nav_order: 2
has_children: true
---

What is it
==========

The common task in many applications is to show badges with a number of updates to the user.
This docker image helps to store and retrieve badges.
Badges uses Mongo database to persist badge events and API written on PHP.

Installation
============

```bash
docker run \
-p 80:80/tcp \
-e MG_HOST=mongo \
-e MG_DATABASE=badges \
-e MG_COLLECTIONS=my_collection1,my_collection2 \
-d images.perfumerlabs.com/dist/badge:v2.0.0
```

Mongo container must be set up before this container startup.

Environment variables
=====================

- BADGES_LIFETIME - lifetime of documents. This parameter sets index with "expireAfterSeconds" option. Optional. "2592000" (1 month) by default.
- MG_HOST - mongo server host. Optional. "mongo" by default.
- MG_PORT - mongo server port. Optional. "27017" by default.
- MG_DATABASE - mongo database. Optional. "badges" by default.
- MG_COLLECTIONS - mongo collection names to use separated by a comma. Badges will setup a number of indexes for these collections. Optional.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 100.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- PHP_MEMORY_LIMIT - memory_limit option in php.ini. Optional. The default value is 128M.

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.

How it works
============

When an event of the user appears in the application you push it to Badges.
Then you can retrieve a list of badges or count them.

Every event has a name. The event name has nested structure.
For example, "foo/bar" is a subevent of "foo".
Same as "foo/bar/baz" is a subevent of "foo/bar" or "foo".

So, if you pushed "foo/bar" and "foo/bar/baz" events, then there are 2 events "foo".
If you delete "foo" event, then "foo/bar" and "foo/bar/baz" events will be also deleted.

### Example of usage

Lets say, you have a page News in the application.
Every time an article is added to news you have to increment a badge with the number of unread articles.

When new article is created you push an event with the name "news/{id}".
When user reads this article you delete the event "news/{id}".
After retrieving a counter of event "news" it shows a number of unread articles.
If user clicks on the button "Read all articles", you delete event "news" and Badges deletes all subsequent badges as well.
After that the counter of "news" will be 0.

### Collections

If you have several types of users with their own badges structure, you can store badges at different collections.

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
