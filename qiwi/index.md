---
layout: default
title: Qiwi Payments
nav_order: 96
has_children: true
---

Qiwi Terminal Payments
======================

This image provides integration with [Qiwi](https://developer.qiwi.com/) terminals.
Qiwi provides possibility to receive payments through their terminals.
In order to have this possibility you must submit contract with them first.

Usage
-----

After submitting contract you have to provide to Qiwi your payment gateway endpoint.
Provide `https://this-image-host/payment` endpoint.
This image has possibility to configure HTTP Basic Authentication with `HTTP_AUTH_USERNAME` and `HTTP_AUTH_PASSWORD` variables,
and it is strongly recommended to do it.
So, you final endpoint can something like `https://http_username:http_password@this-image-host/payment`,
where `http_username` and `http_password` are HTTP Basic Authentication credentials.

Then Qiwi will send requests to that endpoint.
This image creates a table in the database, called `qiwi_http_log`.
This table consist of all incoming HTTP requests.
Table structure is:

- id[bigint] - auto-incremented identity
- ip[string] - IP-address of sender
- url[string] - URL which was called by sender
- method[string] - request method, for example, GET or POST
- headers[json] - request headers object
- body[string] - request body content
- created_at[datetime] - UTC timestamp of the request

Check Command
-------------

Qiwi has option to validate accounts before payment for validation.
When customer dial his account, Qiwi sends CHECK command to backend.
If you don't want to check accounts and accept any account, then you can just leave default settings,
and the image will do all processing on its side.

If you want to validate accounts on your backend, then set variable `QIWI_CHECK_URL`.
It must be accessible from container endpoint.
When CHECK command received, container will request this `QIWI_CHECK_URL` with GET method and add account number to query parameter.
For example:

If `QIWI_CHECK_URL=http://my-backend/my-check-endpoint`, then the request will be
`GET http://my-backend/my-check-endpoint?account=123456`, where 123456 is customers dialed account.

If account is valid then you must return `200 Ok` status code and any content.
Any other response is considered as invalid and container returns error to Qiwi, then payment is interrupted.

Payment
-------

Container processes payment on its side and doesn't require any action.
This image creates a table in the database to view completed transactions, called `qiwi_transaction`.
Table structure is:

- id[bigint] - auto-incremented identity
- account[string] - account of customer
- code[string] - unique identity of transaction in Qiwi system.
- sum[float] - sum of payment.
- status[integer] - status of transaction (0 - transaction received, 1 - transaction completed (see below about webhooks)

Webhook
-------

If you want to receive events about payment to your backend, then you have to configure `QIWI_WEBHOOK_URL`.
It must be accessible from container endpoint.
After container receive a transaction, it is saved with status 0 (received).
On the background container will make repetitive request to `QIWI_WEBHOOK_URL` with POST method till valid response is received.
Note, that there are no retry limit.
For example:

If `QIWI_WEBHOOK_URL=http://my-backend/my-webhook-endpoint`, then the request wll be:

`POST http://my-backend/my-webhook-endpoint`

```json
{
    "id": "123",
    "account": "123456",
    "code": "563456456745",
    "sum": 45.0,
    "created_at": "2022-02-10 12:00:00"
}
```

where

- "id" - unique identity in the container
- "code" - unique identity in Qiwi system
- "account" - account of customer
- "sum" - sum of payment
- "created_at" - timestamp of operation

You must respond with any 2xx status code and any content. Any other response will be considered as invalid,
and request will be retried later.
Note, that there can be several requests with same "code", so you have to properly process it on your side.