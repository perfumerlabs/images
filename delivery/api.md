---
layout: default
parent: Delivery
title: API
nav_order: 4
---

API Reference
=============

### Create a delivery

`POST /delivery`

Parameters (json):

- min, max, gap [integer,required] - parameters for send fraction task to Queue
- name [string,required] - delivery name
- has_email [boolean,optional] - delivery has email notification?. Default is false
- has_sms [boolean,optional] - delivery has sms notification?. Default is false
- has_feed [boolean,optional] - delivery has feed notification?. Default is false
- email_subject [array,optional] - subject of email notification with translations. Format

```json
{
  "en": "Hello!"
}
```

- email_html [array,optional] - HTML body of email notification with translations. Format

```json
{
  "en": "<html><body><p>This is test email</p></body></html>"
}
```

- sms_message [array,optional] - Message of SMS notification with translations. Format

```json
{
  "en": "This is test SMS"
}
```

- feed_title [array,optional] - Title of feed notification with translations. Format

```json
{
  "en": "Hello!"
}
```

- feed_text [array,optional] - Title of feed notification with translations. Format

```json
{
  "en": "This is test notification"
}
```

- feed_image [array,optional] - Image of feed notification with translations. Format

```json
{
  "en": "http://example.com/file/123"
}
```

- feed_payload [array,optional] - Payload of feed notification. Example

```json
{
  "to": "http://example.com/notification/1"
}
```

- payload [array,optional] - Some additional data. Example

```json
{
  "one": "one",
  "two": "two"
}
```

- data_url [string, required] - for fetching users requisites, e.g. emails, phones, feed collections and feed
  recipients. Expected response format:

```json
{
  "content": [
    {
      "email": "email@example.com",
      "phone": "77777777777",
      "feed_collection": "users_collection",
      "feed_recipient": "2",
      "locale": "en"
    },
    {
      "email": "email2@example.com",
      "feed_collection": "users_collection",
      "feed_recipient": "3",
      "locale": "ru"
    },
    {
      "email": "email3@example.com",
      "phone": "77777777555",
      "locale": "en"
    }
  ]
}
```

- filters [array,required] - filters using with data_url. Example:

```json
{
  "code": "some_code",
  "type": "some_type"
}
```

Response example:

```json
{
  "status": true
}
```

### Update a delivery

`PATCH /delivery`

Parameters (json):

- id [integer,required] - id of updating delivery
- min, max, gap [integer,required] - parameters for send fraction task to Queue
- name [string,required] - delivery name
- has_email [boolean,optional] - delivery has email notification?. Default is false
- has_sms [boolean,optional] - delivery has sms notification?. Default is false
- has_feed [boolean,optional] - delivery has feed notification?. Default is false
- email_subject [array,optional] - subject of email notification with translations. Format

```json
{
  "en": "Hello!"
}
```

- email_html [array,optional] - HTML body of email notification with translations. Format

```json
{
  "en": "<html><body><p>This is test email</p></body></html>"
}
```

- sms_message [array,optional] - Message of SMS notification with translations. Format

```json
{
  "en": "This is test SMS"
}
```

- feed_title [array,optional] - Title of feed notification with translations. Format

```json
{
  "en": "Hello!"
}
```

- feed_text [array,optional] - Title of feed notification with translations. Format

```json
{
  "en": "This is test notification"
}
```

- feed_image [array,optional] - Image of feed notification with translations. Format

```json
{
  "en": "http://example.com/file/123"
}
```

- feed_payload [array,optional] - Payload of feed notification. Example

```json
{
  "to": "http://example.com/notification/1"
}
```

- payload [array,optional] - Some additional data. Example

```json
{
  "one": "one",
  "two": "two"
}
```

Response example:

```json
{
  "status": true
}
```

### Get a delivery

`GET /delivery`

Parameters (json):

- id [integer,required] - id of delivery

Response example:

```json
{
  "status": true,
  "content": {
    "delivery": {
      "id": 1,
      "name": "New delivery name",
      "has_email": true,
      "has_sms": true,
      "has_feed": true,
      "status": "started",
      "progress": 0.1,
      "created_at": "2020-03-20 15:00:00",
      "updated_at": "2020-03-20 16:00:00",
      "data_url": "http://example.com/users",
      "filters": {
        "code": "some_code",
        "type": "some_type"
      },
      "email_subject": {
        "en": "Hello!"
      },
      "email_html": {
        "en": "<html><body><p>This is test email</p></body></html>"
      },
      "sms_message": {
        "en": "This is test SMS"
      },
      "feed_title": {
        "en": "Hello!"
      },
      "feed_text": {
        "en": "This is test notification"
      },
      "feed_image": {
        "en": "http://example.com/file/123"
      },
      "feed_payload": {
        "to": "http://example.com/notification/1"
      },
      "payload": {
        "one": "one",
        "two": "two"
      }
    }
  }
}
```

### Get a deliveries collection

`GET /deliveries`

Parameters (json):

- limit, offset [integer,optional] - LIMIT and OFFSET allow you to retrieve just a portion of the rows that are
  generated by the rest of the query
- count [boolean,optional] - show total count? Default is false
- name [string,optional] - name of the delivery
- has_email [boolean,optional] - if true return only deliveries with email notifications, if false - only without email
  notifications, if null - both. Default is "null".
- has_sms [boolean,optional] - if true return only deliveries with sms notifications, if false - only without sms
  notifications, if null - both. Default is "null".
- has_feed [boolean,optional] - if true return only deliveries with feed notifications, if false - only without feed
  notifications, if null - both. Default is "null".
- status [string,optional] - status of the delivery. Possible variants: "started", "finished", "canceled". Default is "
  null".

Response example:

```json
{
  "status": true,
  "content": {
    "deliveries": [
      {
        "id": 1,
        "name": "New delivery name",
        "has_email": true,
        "has_sms": true,
        "has_feed": true,
        "status": "started",
        "progress": 0.1,
        "created_at": "2020-03-20 15:00:00",
        "updated_at": "2020-03-20 16:00:00"
      },
      {
        "id": 2,
        "name": "New delivery name 2",
        "has_email": true,
        "has_sms": true,
        "has_feed": true,
        "status": "started",
        "progress": 0.3,
        "created_at": "2020-03-21 15:00:00",
        "updated_at": "2020-03-22 16:00:00"
      }
    ]
  }
}
```

### Delete a delivery

`DELETE /delivery`

Parameters (json):

- id [integer,required] - id of delivery

Response example:

```json
{
  "status": true
}
```

### Cancel a delivery

`POST /delivery/cancel`

Parameters (json):

- id [integer,required] - id of delivery

Response example:

```json
{
  "status": true
}
```