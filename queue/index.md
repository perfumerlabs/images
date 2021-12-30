---
layout: default
title: Queue
nav_order: 100
has_children: true
---

What is it
==========

This is container providing queueing.
Queueing is a common task for web applications.
Queue responsibility is to register a task and execute it at particular time.
Another responsibility is to normalize load to your servers.

This Queue works in terms of HTTP-requests. Firstly, you register a task via REST API.
You can provide delay or datetime request parameter to tell Queue, when to execute the task.
Then, in this time Queue makes HTTP-request with parameters you provided to your backend.

Queue uses [Tarantool](https://www.tarantool.io/) NoSQL database to store tasks.
Tarantool has a number of extensions and `queue` extension is one of them.
Tarantool is already bundled to this image, so you don't need to make any additional administration.

Workers
=======

A worker is an instance of daemon which does 1 task at a time.
Worker gets a task from Tarantool, executes it, waits until backend responds, then takes new task.
So, if you want to execute more tasks at a time, configure more workers.
Be careful not to set too many workers, because this may lead to overload your backend.

Every worker has a name. The `name` is purpose this worker exists for.
Often it is needed to have multiple types of workers or several workers of same type.
For example, you send sms via some provider and they restricts API calls to 3 requests per second.
In this case you have to use dedicated queue worker with 333 milliseconds freeze time between tasks.
Or, for instance, you send push notifications.
In this case you should configure multiple workers of same type, because single worker will handle tasks too slow.

Queue supports configuring any number of workers and its types.
For example, we want to have 3 workers dealing with emails sending, 2 workers dealing with sms sending.
To configure it provide QUEUE_WORKERS environment variable in json format like this:

```
-e "QUEUE_WORKERS={\"email\":3,\"sms\":2}"
```

Queue automatically configures Tarantool database to have 2 tables for queues and start 5 worker daemons.
Always divide your workers to different groups to do same kind of activity.

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
