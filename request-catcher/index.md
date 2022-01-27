---
layout: default
title: Request Catcher
nav_order: 105
has_children: true
---

Request Catcher
===============

Request Catcher is a tool for catching web requests for testing webhooks, http clients and other applications that communicate over http.
Especially it is very helpful to cover it with autotests.

How to view catched requests
----------------------------

If you properly installed container, then you should lead traffic to Request Catcher by specifying its hostname in needed place.

Request Catcher prints all incoming request data to container stdout.

For example, Request Catcher received following request:

`POST http://request-catcher/some/url?a=1&b=2` with json body and header:

`X-My-Header: value`

```json
{
    "foo": "bar"
}
```

Then Request Catcher prints to stdout following output:

```json
{
    "host": "request-catcher",
    "port": 80,
    "path": "/some/url",
    "headers": {
        "x-my-header": "value"
    },
    "query": {
        "a": 1,
        "b": 2
    },
    "json": {
        "foo": "bar"
    },
    "body": "{\"foo\": \"bar\"}"
}
```

Then you can view the request and easily debug, what your application does wrong.

Request Catcher in autotests
----------------------------

We especially recommend to use Request Catcher to cover your application API with tests.

For example: assume you have a containerized service which sends requests to some external API and you want to cover tests for it.

```yml
version: '2.2'
services:
  my-service:
    image: my-service
```

Assume that `my-service` sends requests to the host `external-api` in the code.
Then just run Request Catcher to respond to `external-api` host requests:

```yml
version: '2.2'
services:
  my-service:
    image: my-service
  external-api:
    image: images.perfumerlabs.com/dist/request-catcher:v2.0.0
```

Every request sent to Request Catcher is persisted and can be asserted in the tests via `/_last_request_` endpoint.
For example, this is pseudocode in some PHP test ([Codeception](https://codeception.com/) framework is used):

```php
class ServiceCest
{
    // Codeception test
    public function tryToTest(\ApiTester $I)
    {
        // send request to your service endpoint
        $I->haveHttpHeader('Content-Type', 'application/json');
        $I->sendPost('/my-endpoint');

        // Consider /my-endpoint internally sends request to POST http://external-api/foo/bar with some json payload

        // Then we can just get last request content from Request Catcher and assert it
        $I->haveHttpHeader('Content-Type', 'application/json');
        $I->sendGet('http://external-api/_last_request_');
        $I->seeResponseCodeIs(\Codeception\Util\HttpCode::OK);
        $I->seeResponseIsJson();
        $I->seeResponseContainsJson([
            'host' => 'external-api',
            'path' => '/foo/bar',
            'method' => 'post',
            'json' => [
                'param1' => 'value1',
                'param2' => 'value2',
            ],
        ]);
    }
}
```

Assert no request received
--------------------------

Sometimes it is needed to check in autotest, that no request is sent.
Then you can delete last request content and then after launching your endpoint check is last request returns 204 status code.
For example, this is pseudocode in some PHP Codeception test:

```php
class ServiceCest
{
    // Codeception test
    public function tryToTest(\ApiTester $I)
    {
        // clear last request, because it can left after previous operations
        $I->haveHttpHeader('Content-Type', 'application/json');
        $I->sendDelete('http://external-api/_last_request_');
        $I->seeResponseCodeIs(\Codeception\Util\HttpCode::OK);

        // send request to your service endpoint
        $I->haveHttpHeader('Content-Type', 'application/json');
        $I->sendPost('/my-endpoint');

        // Then assert that "_last_request_" returns "204 No Content" status code
        $I->haveHttpHeader('Content-Type', 'application/json');
        $I->sendGet('http://external-api/_last_request_');
        $I->seeResponseCodeIs(\Codeception\Util\HttpCode::NO_CONTENT);
    }
}
```