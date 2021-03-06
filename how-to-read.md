---
layout: default
title: How to Read the Docs
nav_order: 3
---

This chapter consists of information about agreements we keep in mind while writing this documentation.

Container Installation
----------------------

When we describe how to install container, we use `docker-compose` config and `docker run` commands.
You should know that this is acceptable only for local or testing developments.
In production environments you should use software like Kubernetes or Rancher.

We write configs via `docker-compose` and `docker run`, because it is very simple protocol to visualize how
environment variables and volumes should work and how containers must integrate between each other.

Don't copy and paste our configs as-is to your production environment.
Make sure to convert installation definitions to your favourite orchestration system configs instead.

Hosts
-----

When describing API we usually use examples like `POST /some-endpoint`.
We intentionally don't write host in the URL, because all images accept any host to 80 port
(unless opposite is mentioned in the image documentation).

Image Tags
----------

None of our images has `latest` tag.
We believe that this is a weird practice.
Always use specific version for image name.

We try to keep up-to-date tag names in the documentation, but we can forget to update docs after releases.
Keep in mind during deployment to check latest tag name in our [Gitlab](https://soft.perfumerlabs.com).
