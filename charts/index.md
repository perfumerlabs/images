---
layout: default
title: Charts
nav_order: 33
has_children: true
---

Charts
======

Charts provides simple and fast way to create chart pictures.
You just construct properly URL parameters and get ready picture.
Under cover all charts are drawn with [Apache Charts library](https://echarts.apache.org/).

Line Chart
----------

Line chart is built via next URL:

```
/pub/chart?x=item1,item2,item3&y=100,200,300
```

where:

- x - comma separated x-axis items;
- y - comma separated y-axis values respectively.

Also there are some additional parameters:

- x_type - type of x-axis items. See [possible parameters](https://echarts.apache.org/en/option.html#xAxis.type). Default is `category`.
- y_type - type of y-axis items. See [possible parameters](https://echarts.apache.org/en/option.html#yAxis.type). Default is `value`.

Bar Chart
---------

Bar chart is built via next URL:

```
/pub/chart?x=item1,item2,item3&y=100,200,300&type=bar
```

Options are the same as at Line Chart.

Pie Chart
---------

Pie chart is built via next URL:

```
/pub/pie?x=item1,item2,item3&y=100,200,300
```

where:

- x - comma separated names of pie parts;
- y - comma separated values of pie parts.

Also there are some additional parameters:

- legend - show pie legend. Default is `true`.
- label - show pie labels. Default is `true`.
- radius - radius in percents of pie. There are inner and outer radius. Type them comma separated. For example, `radius=10,40` will draw a ring with inner radius 10% and outer 40%.

Custom Chart
------------

If you want to draw fully customized chart, you can provide chart config with 3 ways.
See config option reference and examples at [Apache Charts](https://echarts.apache.org/) page.

```
/pub/chart?url=<URL of config>
```

where `url` is web-endpoint which returns JSON config.

```
/pub/chart?query_string=<url-encoded-config>
```

where `query_string` is config object formed as form-data query string object.

```
/pub/chart?json=<url-encoded-json>
```

where `json` is url-encoded JSON object.