---
layout: default
parent: ElasticSearch Search-as-you-type
title: API
nav_order: 4
---

API Reference
=============

### Create an index

`POST /index`

Parameters (json):
- name [string,required] - name of the index.

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

### Delete an index

`DELETE /index`

Parameters (json):
- name [string,required] - name of the index.

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

### Create a document

`POST /document`

Request parameters (json):
- index [string,required] - name of the index.
- code [string,required] - unique identity of inserted content.
- text [string,required] - text of document.

Request example:

```json
{
    "index": "foobar",
    "code": "1",
    "text": "World"
}
```

Response example:

```json
{
    "status": true
}
```

### Create multiple documents at once

`POST /documents`

Request parameters (json):
- index [string,required] - name of the index.
- documents [array,required] - array of documents to save.
- documents.code [string,required] - unique identity of inserted content.
- documents.text [string,required] - text of document.

Request example:

```json
{
    "index": "foobar",
    "documents": [
        {
            "code": "1",
            "text": "World"
        }
    ]
}
```

Response example:

```json
{
    "status": true
}
```

### Delete a document

`DELETE /document`

Request parameters (json or URL):
- index [string,required] - index of elasticsearch.
- code [string,required] - unique identity of inserted content.

Request example:

```json
{
    "index": "foobar",
    "code": "1"
}
```

Response example:

```json
{
    "status": true
}
```

### Search documents

`GET /documents`

Request parameters (json):
- index [string,required] - index of elasticsearch.
- search [string,required] - search string in title or text.
- from [string,optional] - offset of returned results set. Default is 0.
- size [string,optional] - size of returned results set. Default is 50.

Request example:

```json
{
    "index": "foobar",
    "search": "lorem ipsum"
}
```

Response parameters (json):
- code [string] - unique identity of inserted content.
- title [string] - title of document.
- text [string] - text of document.

Response example:

```json
{
    "status": true,
    "content": {
        "documents": [
            {
                "code": "1",
                "text": "World"
            }
        ]
    }
}
```