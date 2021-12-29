---
layout: default
title: Queue
nav_order: 10
has_children: true
---

Queue provides scheduled execution of tasks.

How it works
============

Queueing is a common task for web applications. Queue responsibility is to register a task and execute it at particular time.

This Queue works in terms of HTTP-requests. Firstly, you register a task via REST API. You can provide delay or datetime request parameter to tell Queue, when to execute the task. Then, in this time Queue makes HTTP-request with parameters you provided to your backend.

Workers
=======

Often it is needed to have multiple types of workers or several workers of same type. For example, you send sms via some provider and they restricts API calls to 3 requests per second. In this case you have to use dedicated queue worker with 333 milliseconds freeze time between tasks. Or, for instance, you send push notifications. In this case you should configure multiple workers of same type, because single worker will handle tasks too slow.

Queue supports configuring any number of workers and its types. For example, we want to have 3 workers with name "foo", 2 workers with name "bar" and 1 - "baz". To configure it provide QUEUE_WORKERS environment variable in json format like this:

```
-e "QUEUE_WORKERS={\"foo\":3,\"bar\":2,\"baz\":1}"
```

Queue automatically configures database to have 3 tables for queues and start 6 workers.

Types of tasks
==============

There are 2 types of tasks currently: regular and fraction.

### Regular task

A regular task is a task that is done as-is: you set parameters and Queue makes HTTP-request with those parameters at particular time.

For example, if we want to send email to particular user, we can create a task like this:

```
POST http://queue/task

{
    "worker": "email",
    "url": "http://my-site.com/send-email",
    "method": "post",
    "delay": 60,
    "json": {
        "subject": "Hello",
        "body": "World",
        "to": "user@example.com"
    }
}
```

And Queue will make an HTTP-request with this set of parameters in 60 seconds to your backend.

### Fraction task

Sometimes it is needed to process a large amount of data. For example, to send emails to millions of users.
In this case you can't even fetch all the IDs of users and send them to Queue.
Fraction task is invented for these types of cases.

When creating fraction task you provide 3 parameters: a minimal supposed bound of data, a maximum supposed bound of data, and a reasonable gap.

For example, suppose we need to send emails to users with ID from 1000 to 1000000 in the database.
We push a task to Queue like this:

```
POST http://queue/fraction

{
    "worker": "email",
    "url": "http://my-site.com/send-email",
    "method": "post",
    "json": {
        "subject": "Hello",
        "body": "World"
    },
    "min": 1000,
    "max": 1000000,
    "gap": 100
}
```

Queue will divide internally the 1000-1000000 segment to small pieces of 100 items
and then send a series of requests to your backend for each piece of data.

First request will be:

```
POST http://my-site.com/send-email

{
    "subject": "Hello",
    "body": "World",
    "_min": 1000,
    "_max": 1099,
    "_gap": 100
}
```

second request will be:

```
POST http://my-site.com/send-email

{
    "subject": "Hello",
    "body": "World",
    "_min": 1100,
    "_max": 1199,
    "_gap": 100
}
```

and so on until `max` parameter reaches 1000000. This scheme allows to spread the load by time.
