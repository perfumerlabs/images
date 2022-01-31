---
layout: default
title: Feed
nav_order: 70
has_children: true
---

Feed Data Structure
===================

This image provides convenient API to manage feed-like data.
It can be notifications feed, simple news feed or other.
Image creates database structure in the PostgreSQL and API to create, update or delete records.

How it works
------------

Consider, you have to make notification feed for users on the site.
Then create a collection on the database:

`POST /collection`

```json
{
    "name": "notifications"
}
```

Feed will create separate table in the database, columns and indices.
Also you can provide collection name to image environment variable `FEED_COLLECTIONS` and it will be auto-created on container initialization.

Then, you can push records to this collection:

`POST /record`

```json
{
    "collection": "notifications",
    "recipient": "user1",
    "title": "Hello",
    "text": "You have a message"
}
```

where `recipient` is string identifier of user on your site.
To receive records call:

`GET /records`

```json
{
    "collection": "notifications",
    "recipient": "user1"
}
```

and it will return recipients records array and you can render them to the site.

Read records
------------

Image has also a Read API. If you call `GET /records`, then response can be something like:

```json
{
    "content": {
        "records": [
            {
                "id": 123,
                "recipient": "user1",
                "title": "Hello",
                "text": "You have a message",
                "created_at": "2022-02-01 12:00:00",
                "is_read": false
            }
        ]
    }
}
```

Notice `is_read` parameter, you can check it and mark this record on the site with red dot or other sign.
To make the record read, call:

`POST /record/read`

```json
{
    "collection": "notifications",
    "id": 123
}
```

Then request returning a record will have `is_read=true`.