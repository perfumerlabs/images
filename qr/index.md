---
layout: default
title: QR generator
nav_order: 98
has_children: true
---

What is it
==========

This is container providing convenient API to generate QR codes from string.

Quick example
-------------

Let's consider you have a string you want to pack to QR code and set it to your site page.
Then you can just form an html link:

```html
<a href="http://qr-host.com/qr?data=http://mysite.com/link"></a>
```

where `qr-host.com` is host to this image container,
`http://mysite.com/link` is a string to encode to QR code.

Note, that all content provided to query parameter `data` must be url-encoded.