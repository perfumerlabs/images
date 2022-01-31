---
layout: default
parent: Feed
title: API
nav_order: 4
---
API Reference
=============

### Create a collection

`POST /collection`

Parameters (json):
- name [string,required] - name of the collection.

Request example:

```json
{
    "name": "foobar"
}
```

Response example:

```json
{
    "status": true
}
```

### Create a record

`POST /record`

Request parameters (json):
- collection [string,required] - name of the collection.
- recipient [string,required] - name of record recipient.
- sender [string,optional] - name of record author.
- thread [string,optional] - additional field for tagging record.
- title [string,optional] - title of record.
- text [string,optional] - text of record.
- image [string,optional] - image of record.
- payload [json,optional] - any JSON-serializable content.
- created_at [datetime,optional] - date of record creation.

Request example:

```json
{
    "collection": "foobar",
    "recipient": "client1",
    "sender": "client2",
    "thread": "chat",
    "title": "Hello",
    "text": "World",
    "image": "https://example.com/image.jpg",
    "payload": {
        "foo": "bar"
    }
}
```

Response parameters (json):
- id [integer] - unique identity of inserted document.
- recipient [string] - name of record recipient.
- sender [string] - name of record author.
- thread [string] - additional field for tagging record.
- title [string] - title of record.
- text [string] - text of record.
- image [string] - image of record.
- payload [json] - any JSON-serializable content.
- created_at [datetime] - date of creation.

Response example:

```json
{
    "status": true,
    "content": {
        "record": {
            "id": 1,
            "recipient": "client1",
            "sender": "client2",
            "thread": "chat",
            "title": "Hello",
            "text": "World",
            "image": "https://example.com/image.jpg",
            "payload": {
                "foo": "bar"
            },
            "created_at": "2020-10-01 00:00:00"
        }
    }
}
```

### Get a record

`GET /record`

Request parameters (json):
- collection [string,required] - name of the collection.
- id [integer,required] - the id of document.

Request example:

```json
{
    "collection": "foobar",
    "id": 1
}
```

Response parameters (json):
- id [integer] - unique identity of inserted document.
- recipient [string] - name of record recipient.
- sender [string] - name of record author.
- thread [string] - additional field for tagging record.
- title [string] - title of record.
- text [string] - text of record.
- image [string] - image of record.
- payload [json] - any JSON-serializable content.
- created_at [datetime] - date of creation.
- is_read [boolean] - whether record read or not.

Response example:

```json
{
    "status": true,
    "content": {
        "record": {
            "id": 1,
            "recipient": "client1",
            "sender": "client2",
            "thread": "chat",
            "title": "Hello",
            "text": "World",
            "image": "https://example.com/image.jpg",
            "payload": {
                "foo": "bar"
            },
            "is_read": false,
            "created_at": "2020-10-01 00:00:00"
        }
    }
}
```

### Delete a record

`DELETE /record`

Request parameters (json or URL):
- collection [string,required] - name of the collection.
- id [integer,required] - the id of document.

Request example:

```json
{
    "collection": "foobar",
    "id": 1
}
```

Response example:

```json
{
    "status": true
}
```

### Get records

`GET /records`

Request parameters (json):
- collection [string,required] - name of the collection.
- recipient [string,required] - name of the recipient.
- sender [string,optional] - name of the sender.
- user [string,optional] - name of the sender or recipient. If user is present, recipient and sender will be ignored.
- thread [string,optional] - name of the thread.
- search [string,optional] - searching string in "title" or "text".
- id [integer,optional] - the id of document to start from.
- order [string,optional] - type of ordering ("asc" or "desc"). Default is "desc".
- is_read [boolean,optional] - if true return only read records, if false - only unread, if null - both. Default is "null".
- limit [integer,optional] - number of records to return.
- offset [integer,optional] - number of records to offset from.

Request example:

```json
{
    "collection": "foobar",
    "recipient": "client1"
}
```

Response parameters (json):
- id [integer] - unique identity of inserted document.
- recipient [string] - name of record recipient.
- sender [string] - name of record author.
- thread [string] - additional field for tagging record.
- title [string] - title of record.
- text [string] - text of record.
- image [string] - image of record.
- payload [json] - any JSON-serializable content.
- created_at [datetime] - date of creation.
- is_read [boolean] - whether record read or not.

Response example:

```json
{
    "status": true,
    "content": {
        "records": [
            {
                "id": 1,
                "recipient": "client1",
                "sender": "client2",
                "thread": "chat",
                "title": "Hello",
                "text": "World",
                "image": "https://example.com/image.jpg",
                "payload": {
                    "foo": "bar"
                },
                "is_read": false,
                "created_at": "2020-10-01 00:00:00"
            }
        ]
    }
}
```

### Get records number by filters

`GET /records/count`

Request parameters (json):
- collection [string,required] - name of the collection.
- recipient [string,required] - name of the recipient.
- sender [string,optional] - name of the sender.
- user [string,optional] - name of the sender or recipient. If user is present, recipient and sender will be ignored.
- thread [string,optional] - name of the thread.
- search [string,optional] - searching string in "title" or "text".
- id [integer,optional] - the id of document to start from.
- is_read [boolean,optional] - if true return only read records, if false - only unread, if null - both. Default is "null".

Request example:

```json
{
    "collection": "foobar",
    "recipient": "client1"
}
```

Response parameters (json):

- records [integer] - number fo found records.

Response example:

```json
{
    "status": true,
    "content": {
        "records": 5
    }
}
```

### Update records

`PATCH /records`

Request parameters (json):
- collection [string,required] - name of the collection.
- where [object,required] - parameter object for WHERE.
- where.recipient [string,optional] - name of the recipient.
- where.sender [string,optional] - name of the sender.
- where.thread [string,optional] - name of the thread.
- where.user [string,optional] - name of the sender or recipient.
- set [object,required] - parameter object for SET.
- set.recipient [string, optional] - name of record recipient.
- set.sender [string, optional] - name of record author.
- set.thread [string, optional] - additional field for tagging record.
- set.user [string, optional] - name of the sender or recipient.
- set.title [string, optional] - title of record.
- set.text [string, optional] - text of record.
- set.image [string, optional] - image of record.
- set.payload [object, optional] - any JSON-serializable content.

Request example:

```json
{
    "collection": "foobar",
    "where": {
        "thread": "foo"
    },
    "set": {
        "sender": "bar",
        "payload": {
             "foo": "bar"
        }
    }
}
```

Response parameters (json):
- status [boolean] - result status.

Response example:

```json
{
    "status": true
}
```

### Read a record

`POST /record/read`

Request parameters (json):
- id [integer,required] - id of the record.
- collection [string,required] - name of the collection.

Request example:

```json
{
    "collection": "foobar",
    "id": 1
}
```

Response example:

```json
{
    "status": true
}
```

### Unread a record

`POST /record/unread`

Request parameters (json):
- id [integer,required] - id of the record.
- collection [string,required] - name of the collection.

Request example:

```json
{
    "collection": "foobar",
    "id": 1
}
```

Response example:

```json
{
    "status": true
}
```

### Read all records of recipient

`POST /records/read`

Request parameters (json):
- recipient [string,required] - name of recipient.
- collection [string,required] - name of the collection.

Request example:

```json
{
    "collection": "foobar",
    "recipient": "client1"
}
```

Response example:

```json
{
    "status": true
}
```

### Delete all records of recipient

`DELETE /records`

Request parameters (json):
- recipient [string,required] - name of recipient.
- collection [string,required] - name of the collection.

Request example:

```json
{
    "collection": "foobar",
    "recipient": "client1"
}
```

Response example:

```json
{
    "status": true
}
```
