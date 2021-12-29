---
layout: default
title: Badges
nav_order: 2
has_children: true
---

What is it
==========

The common task in many applications is to show badges with a number of updates to the user.
This docker image helps to store and retrieve badges.
Badges uses Mongo database to persist badge events and API written on PHP.

How it works
============

When an event of the user appears in the application you push it to Badges.
Then you can retrieve a list of badges or count them.

Every event has a name. The event name has nested structure.
For example, "foo/bar" is a subevent of "foo".
Same as "foo/bar/baz" is a subevent of "foo/bar" or "foo".

So, if you pushed "foo/bar" and "foo/bar/baz" events, then there are 2 events "foo".
If you delete "foo" event, then "foo/bar" and "foo/bar/baz" events will be also deleted.

### Example of usage

Lets say, you have a page News in the application.
Every time an article is added to news you have to increment a badge with the number of unread articles.

When new article is created you push an event with the name "news/{id}".
When user reads this article you delete the event "news/{id}".
After retrieving a counter of event "news" it shows a number of unread articles.
If user clicks on the button "Read all articles", you delete event "news" and Badges deletes all subsequent badges as well.
After that the counter of "news" will be 0.

### Collections

If you have several types of users with their own badges structure, you can store badges at different collections.
