---
layout: default
title: PDF converter
nav_order: 95
has_children: true
---

What is it
==========

This is container providing convenient API to generate PDF files from HTML source.

Quick example
-------------

Let's consider you have a link on your site, which returns HTML content.
Then you can form a link, which returns PDF document:

```html
<a href="http://pdf-host.com/pdf?html_link=http://mysite.com/url-returns-html"></a>
```

where `pdf-host.com` is host to this image container,
`http://mysite.com/url-returns-html` is a link on your site returning HTML.

You can also provide plain HTML to `html` parameter:

```html
<a href="http://pdf-host.com/pdf?html=<p>Lorem ipsum dolor sit amet</p>"></a>
```

or to API:

`GET http://pdf-host.com/pdf`

```json
{
  "html": "<p>Lorem ipsum dolor sit amet</p>"
}
```

Note, that all content provided to query parameter `html` or `html_link` must be url-encoded.