---
layout: default
parent: Feed
title: Database tables
nav_order: 5
---

Database tables
===============

After setup there are 1 predefined table in database:

### feed_collection

Registry of collections. Fields:

- name [string] - Name of collection

### feed_data_*

Table of collection records. Its name depends on collection name.
For example, if collection is named with "foo", then this tables name will be "feed_data_foo".
Fields:

- id[bigint] - autoincrement identifier
- recipient[string] - recipient of the record
- sender[string] - optional sender of the record
- thread[string] - optional extra field to differentiate record meaning
- title[string] - title of the record
- text[string] - title of the record
- image[string] - image link of the record
- payload[object] - any optional not-searchable payload to provide to and to get then from the record, i.e. colours, styling, additional meta data, etc.
- created_at[datetime] - DateTime of record creation
- is_read[bool] - is a record is read by recipient