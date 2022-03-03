---
layout: default
parent: Notifications Center
title: API
nav_order: 4
---

API Reference
=============

### Create notification

`POST /notification`

Parameters (json):
- name [string,required] - name of notification for reference.
- description [string,optional] - optional short description.
- code [string,required] - unique code which will be used to send notify signal.
- has_email [bool,optional] - whether or not to send email on this notification.
- has_sms [bool,optional] - whether or not to send SMS on this notification.
- has_feed [bool,optional] - whether or not to send feed content on this notification.
- email_subject [string,optional] - email subject.
- email_html [string,optional] - email html.
- sms_message [integer,optional] - sms message.
- feed_title [string,optional] - feed record title.
- feed_text [string,optional] - feed record text.
- feed_image [string,optional] - feed record image.

### Update notification

`PATCH /notification`

Parameters (json):
- id [integer,required] - ID of notification to search for.
- name [string,optional] - name of notification for reference.
- description [string,optional] - optional short description.
- code [string,optional] - unique code which will be used to send notify signal.
- has_email [bool,optional] - whether or not to send email on this notification.
- has_sms [bool,optional] - whether or not to send SMS on this notification.
- has_feed [bool,optional] - whether or not to send feed content on this notification.
- email_subject [string,optional] - email subject.
- email_html [string,optional] - email html.
- sms_message [integer,optional] - sms message.
- feed_title [string,optional] - feed record title.
- feed_text [string,optional] - feed record text.
- feed_image [string,optional] - feed record image.

### DELETE notification

`DELETE /notification`

Parameters (json):
- id [integer,required] - ID of notification to search for.

### Get notifications list

`POST /notifications`

Parameters (json):
- name [string,optional] - name filter.
- code [string,optional] - code filter.
- has_email [bool,optional] - has_email filter.
- has_sms [bool,optional] - has_sms filter.
- has_feed [bool,optional] - has_feed filter.
- limit [integer,optional] - limit returned items.
- offset [integer,optional] - offset of returned items.
- count [bool,optional] - whether or not to count items with specified filters.

### Send notification

`POST /notify`

Parameters (json):
- notification [string,required] - code of notification to send.
- locale [string,required] - locale of notification.
- email [string,optional] - target email.
- phone [string,optional] - target phone.
- feed_collection [string,optional] - target feed collection.
- feed_recipient [string,optional] - target feed recipient.
- feed_payload [object,optional] - optional feed payload.
- payload [object,optional] - object of data to replace in template.