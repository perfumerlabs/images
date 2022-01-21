---
layout: default
title: Request Catcher
nav_order: 105
has_children: true
---

What is it
==========

This is a tool for catching web requests for testing webhooks, http clients and other applications that communicate over http.

Also it is very helpful in autotests.
For example: assume you have a containerized service which sends requests to some external API and you want to cover tests for it.

```yml
version: '2.2'
services:
  my-service:
    image: my-service
```

Assume that `my-service` sends requests to the host `external-api` in the code.
Then just run RequestCatcher to respond to `external-api` host requests:

```yml
version: '2.2'
services:
  my-service:
    image: my-service
  external-api:
    image: images.perfumerlabs.com/dist/request-catcher:v1.0.0
```

Every request sent to RequestCatcher is persisted and can be asserted in the tests via `/_last_request_` endpoint.
For example, this is pseudocode in some php test ([Codeception](https://codeception.com/) framework is used):

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

        // Then we can get last request content from RequestCatcher and assert it
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