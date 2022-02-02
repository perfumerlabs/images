---
layout: default
title: ElasticSearch Search-as-you-type
nav_order: 60
has_children: true
---

ElasticSearch Search-as-you-type
================================

This is microservice to store to and retrieve results from ElasticSearch.
Index mapping in this container is optimized for Search-as-you-type searching.

Pros:

- Regular REST API.
- No need to learn complicated ElasticSearch syntax.
- Index settings optimized for Search-as-you-type search strategy.

### Quickstart

Install your services following [installation guide](/images/es-sayt/install).

Create index:

```
POST /index

{
    "name": "my_index"
}
```

Push a document to index:

```
POST /document

{
    "index": "my_index",
    "code": "unique_code",
    "text": "Lorem ipsum dolor sit amet"
}
```

Search documents in index:

```
GET /documents

{
    "index": "my_index",
    "search": "ipsum"
}
```

Search will be done against both "title" and "text". Will return:

```json
{
    "status": true,
    "content": {
        "documents": [
            {
                "code": "unique_code",
                "text": "Lorem ipsum dolor sit amet"
            }
        ]
    }
}
```
