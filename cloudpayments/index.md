---
layout: default
title: Cloudpayments
nav_order: 35
has_children: true
---

Cloudpayments Payments
======================

This image provides integration with [Cloudpayments](https://cloudpayments.ru/) payments.
In order to have this possibility you must submit contract with them first.

Merchant cabinet
----------------

After submitting contract you have to provide to Cloudpayments your payment gateway endpoint.
Go to [Merchant cabinet](https://merchant.cloudpayments.ru/), choose your site config:

- Enable `Check` notification, if you want to check accounts against your database. Set "GET https://container-host/check" endpoint for this.
- Enable `Pay` notification, it is required. Set "GET https://container-host/pay" endpoint for this.

Container installation
----------------------

Install container with following [instructions](/images/cloudpayments/install).

- Copy `Public ID` from Merchant Cabinet and set to `CLOUDPAYMENTS_PUBLIC_ID` variable.
- Set `CLOUDPAYMENTS_CHECK_URL` to your site's endpoint, if account checks is needed.
- Set `CLOUDPAYMENTS_WEBHOOK_URL` to your site's endpoint, if passing payment information is needed.

Payment page
------------

Image has built-in page to render credit card form.
This is a regular card widget of Cloudpayments and no changes can be done.

It is available on the `https://container-host/pub/card` or `https://container-host/_cloudpayments_/pub/card`.
You should redirect your users to this page in order to make a payment.

Available parameters for URL:

- account[string] - account of customer. Required.
- sum[float] - amount of money to charge. Required.
- description[string] - description about order. Required.
- currency[string] - currency of charge. Required.
- success_url[string] - URL to redirect user after successful payment. Optional.
- fail_url[string] - URL to redirect user after failed payment. Optional.

Example of URL:

`https://container-host/pub/card?account=1234&sum=25&description=HelloWorld&currency=usd&success_url=https://example.com/success`

Accounts Checking
-----------------

Cloudpayments has option to validate accounts before payment.
When customer process payment with specified account, Cloudpayments sends CHECK command to backend.
If you don't want to check accounts and accept any account, then you can just leave default settings,
and the image will do all processing on its side.

If you want to validate accounts on your backend, then set variable `CLOUDPAYMENTS_CHECK_URL`.
It must be accessible from container endpoint.
When CHECK command received, container will request this `CLOUDPAYMENTS_CHECK_URL` with GET method and add account number to query parameter.
For example:

If `CLOUDPAYMENTS_CHECK_URL=http://my-backend/my-check-endpoint`, then the request will be
`GET http://my-backend/my-check-endpoint?account=123456`, where 123456 is customers dialed account.

If account is valid then you must return `200 Ok` status code and any content.
Any other response is considered as invalid and container returns error to Cloudpayments, then payment is interrupted.

Note, this feature requires `Check` notification enabled in Merchant cabinet.

Requests logging
----------------

This image creates a table in the database, called `cloudpayments_http_log`.
This table consist of all incoming HTTP requests to container.
You can use this information to debug.
Table structure is:

- id[bigint] - auto-incremented identity
- ip[string] - IP-address of sender
- url[string] - URL which was called by sender
- method[string] - request method, for example, GET or POST
- headers[json] - request headers object
- body[string] - request body content
- created_at[datetime] - UTC timestamp of the request

Payment
-------

Container processes payment on its side and doesn't require any additional configuration.
Note, container requires `Pay` notification enabled in Merchant cabinet.

This image creates a table in the database to view completed transactions, called `cloudpayments_transaction`.
Table structure is:

- id[bigint] - auto-incremented identity
- account[string] - account of customer
- code[string] - unique identity of transaction in Cloudpayments system.
- sum[float] - sum of payment.
- status[integer] - status of transaction (0 - transaction received, 1 - transaction completed (see below about webhooks)
- created_at[datetime] - UTC timestamp of operation

Webhook
-------

If you want to receive events about payment to your backend, for example, to replenish internal balance of customer,
then you have to configure `CLOUDPAYMENTS_WEBHOOK_URL`.
It must be accessible from container endpoint.
After container receive a transaction, it is saved with status 0 (received).
On the background container will make repetitive requests to `CLOUDPAYMENTS_WEBHOOK_URL` with POST method till valid response is received.
Note, that there are no retry limit.
For example:

If `CLOUDPAYMENTS_WEBHOOK_URL=http://my-backend/my-webhook-endpoint`, then the request wll be:

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

You must respond with any 200 or 201 status code and any content. Any other response will be considered as invalid,
and request will be retried later.
Note, that there can be several requests with same "code", so you have to properly process it on your side.

Container creates a special table in database `cloudpayments_webhook_log`.
This table consists of all webhook requests and responses.
This table can be used to debug webhooks to your backend.
