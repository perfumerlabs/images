---
layout: default
title: Telegram Bridge
nav_order: 115
has_children: true
---

THIS IMAGE IS ON DEVELOPMENT.
THIS IS JUST ANNOUNCEMENT.

Please, contact us to [support@perfumerlabs.com](mailto:support@perfumerlabs.com) if you want to get this container.
This will encourage us to speed up its development.
We will try to prioritize this task and pack it as soon as possible.

Telegram Bridge
===============

This image deals with all communication between your backend and Telegram Bot.
Every time you create Telegram Bot and try to implement features, you face with same problems.
This image aims to simplify routine and help to focus on features.

Saving Messages
---------------

First thing this image will do is to save all incoming and outcoming messages.

Chat Ids
--------

Every Telegram account has its own internal identificator called Chat ID.
You can not obtain it in Telegram Profile.
It can be received only by API method.

Image will save all incoming from Telegram Bot Chat Ids and then you can appoint your applications username
to that Chat ID. Thus, in API you will work with your internal usernames, while this image will convert username to Chat ID
and route message to proper Telegram account.

Chat State
----------

If you create a bot, for example, which asks user for first name and then for last name (in next message),
you have to save somewhere conversation state, because before you ask a user for last name, you should know, if
user has already asked to first question about first name.

This image will preserve all states with different chats, supports resetting state if user doesn't do any action for a long time, etc.

Templating
----------

Telegram supports to render buttons to improve user experience.
Instead of learning telegram docs for this functionality, this image will provide simplified REST API
to create buttons groups for often use-cases.