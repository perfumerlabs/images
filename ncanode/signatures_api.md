---
layout: default
parent: Ncanode
title: Signatures API
nav_order: 5
---

Signatures Persist API Reference
=============

`GET /signature</code>` - get signature

Parameters (json):
- document [string,required] - your document code
- thread [string,required] - additional attribute, when same document is signed in different stories

Success response:

```json
{
  "status": true,
  "message": null,
  "content": {
    "signature": {
      "id": 1,
      "version": 3,
      "document": "doc_12",
      "thread": "ticket_42",
      "signature": "MIIIrwYJKoZIhvcNAQcCoIIIoDCCCx+EWy11vQtlLdPQ==",
      "version_created_by": "some_user",
      "version_created_at": "2021-02-19 15:00:00",
      "version_comment": "some comment",
      "tags": [
        "customer_1",
        "process_1"
      ],
      "created_at": "2021-02-18 15:00:00",
      "updated_at": "2021-02-18 15:00:00"
    }
  }
}
```

`POST /signature` - save signature

Parameters (json):
- document [string,required] - your document code
- thread [string,required] - additional attribute, when same document is signed in different stories
- version [int, required] - version of signature to sign
- cms [string, required] - CMS or XML of signed data
- version_created_by [int,optional] - specify a user, who created signature, if needed
- version_comment [int,optional] - optional comment to signature
- tags [array, optional] - array of tags

Success response:

```json
{
  "status": true,
  "message": null,
  "content": {
    "signature": {
      "id": 2,
      "version": 1,
      "document": "doc_12",
      "thread": "ticket_42",
      "signature": "MIIIrwYJKoZIhvcNAQcCoIIIoDCCCx+EWy11vQtlLdPQ==",
      "version_created_by": "some_user",
      "version_created_at": "2021-02-19 15:00:00",
      "version_comment": "some comment",
      "tags": [
        "customer_1",
        "process_1"
      ],
      "created_at": "2021-02-18 15:00:00",
      "updated_at": "2021-02-18 15:00:00"
    }
  }
}
```

`DELETE /signature` - delete signature and its chain

Parameters (json):
- document [string,required] - your document code
- thread [string,required] - additional attribute, when same document is signed in different stories

Success response:
```json
{
  "status": true
}
```

`GET /signatures` - get signatures

Parameters (json):
- document [string,optional] - sign document
- chain [string,optional] - chain of signature
- stage [string,optional] - stage of signature
- tags [array,optional] - array of tags
- limit [integer,optional] - limit of fetching data
- offset [integer,optional] - offset of fetching data
- count [bool,optional] - show total count?

Success response:
```json
{
  "status": true,
  "message": null,
  "content": {
    "signatures": [
      {
        "id": 2,
        "document": "doc_12",
        "chain": "ticket_42",
        "stage": "stage_1",
        "signature": "MIIIrwYJKoZIhvcNAQcCoIIIoDCCCx+EWy11vQtlLdPQ==",
        "parent": null,
        "tags": [
          "customer_1",
          "process_1"
        ],
        "created_at": "2021-02-18 15:00:00",
        "updated_at": "2021-02-18 15:00:00"
      },
      {
        "id": 1,
        "document": "doc_12",
        "chain": "ticket_42",
        "stage": "stage_1",
        "signature": "MIIIrwYJKoZIhvcNAQcCoIIIoDCCCx+EWy11vQtlLdPQ==",
        "parent": null,
        "tags": [
          "customer_1",
          "process_1"
        ],
        "created_at": "2021-02-18 15:00:00",
        "updated_at": "2021-02-18 15:00:00"
      }
    ]
  }
}

```