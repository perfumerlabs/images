---
layout: default
parent: Email
title: Configuration
nav_order: 3
---

Environment variables
=====================

- SMTP_HOST - SMTP server host. Required.
- SMTP_PORT - SMTP port. Default is 25.
- SMTP_USERNAME - SMTP user login. Optional.
- SMTP_PASSWORD - SMTP user password. Optional.
- SMTP_ENCRYPTION - SMTP encryption method. "ssl" or "tls". Optional.
- SMTP_TIMEOUT - Timeout for sending message. Default is 30.
- EMAIL_FROM - email which is used to set as "FROM". Required.
- EMAIL_SIGNATURE_HTML - text of signature to append to every `html/text` email. Optional.
- EMAIL_SIGNATURE_TEXT - text of signature to append to every `plain/text` email. Optional.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.
- DEBUG - set to "true", if you want to see more debug information in container STDOUT. Default is "false".

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
