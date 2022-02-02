---
layout: default
title: ElasticSearch Fulltext
nav_order: 60
has_children: true
---

ElasticSearch Fulltext
======================

This is microservice to store to and retrieve results from ElasticSearch.
Index mapping in this container is optimized for Fulltext searching.

Pros:

- Regular REST API.
- No need to learn complicated ElasticSearch syntax.
- Index settings optimized for fulltext search strategy.

### Quickstart

Install your services following [installation guide](/images/es-fulltext/install).

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
    "title": "Hello",
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
                "title": "Hello",
                "text": "Lorem ipsum dolor sit amet"
            }
        ]
    }
}
```

### Locales

By default Fulltext is configured to map index to English language.
If you want to reconfigure it to another, provide "stemmer" and "stopwords" parameters during index creation:

```
POST /index

{
    "name": "my_index"
    "stemmer": "russian",
    "stopwords": "_russian_"
}
```

[Stemmer](https://www.elastic.co/guide/en/elasticsearch/reference/current/stemming.html#dictionary-stemmers) is a special filter to process of reducing a word to its root form.
We use [algorithmic stemmers](https://www.elastic.co/guide/en/elasticsearch/reference/current/stemming.html#algorithmic-stemmers).
List of available stemmers for different languages can be found [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-stemmer-tokenfilter.html).

[Stopwords](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-stop-tokenfilter.html) is a dictionary of useless for searching words we desire to remove from query.
List of available stopwards for different languages can be found [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-stop-tokenfilter.html).
