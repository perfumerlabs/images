---
layout: default
parent: PDF converter
title: API
nav_order: 4
---

API Reference
=============

### Create PDF from link returning HTML

`GET /pdf`

Parameters (query or json):
- html_link [string,required] - link to returning HTML endpoint.

Request example:

```html
<a href="http://pdf-host.com/pdf?html_link=http://mysite.com/url-returns-html"></a>
```

or

```json
{
    "html_link": "http://mysite.com/url-returns-html"
}
```

### Create PDF from HTML

`GET /pdf`

Parameters (query or json):
- html [string,required] - HTML content.

Request example:

```html
<a href="http://pdf-host.com/pdf?html=<p>Lorem ipsum dolor sit amet</p>"></a>
```

or

```json
{
    "html": "<p>Lorem ipsum dolor sit amet</p>"
}
```

Note, that all content provided to query parameter `html` or `html_link` must be url-encoded.