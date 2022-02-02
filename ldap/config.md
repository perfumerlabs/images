---
layout: default
parent: LDAP authentication
title: Configuration
nav_order: 3
---

Environment variables
=====================

- LDAP_HOST - LDAP server hostname. Required.
- LDAP_PORT - LDAP server port number. Optional. Default is 389.
- LDAP_BIND_DN - accounts distinguished name template to bind to. Use special "{{ "{{username" }}}}" substitution to specify where to set "username" request parameter. Required.
- LDAP_ENCRYPTION - if connection to LDAP server is SSL-secured (LDAPS), set "ssl" or "tls". Optional. Default is "none".
- LDAP_UNTRUSTED_CERT - if connection to LDAP server is SSL-secured and self-signed certificate is used, set "true". Optional. Default is "false".

Available environment variables for PHP are [here](/images/software.html#php-configuration).

Volumes
=======

This image has no volumes.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
