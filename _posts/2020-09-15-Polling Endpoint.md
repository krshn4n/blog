---
layout: post
title: The Polling endpoint
categories: []
tags: serviceapi logic-apps
status: publish
type: post
published: true
meta: {}
---

Logic apps triggers were discussed in a [previous post](http://hprm.info/Logic-Apps-Teams_Integration/), these triggers rely on the polling endpoint in the ServiceAPI. The polling endpoint is simply a special purpose search designed to support the request/response pattern used by triggers. This pattern expects a response containing records created or updated since the last request.

### triggerState

While the polling endpoint supports most of the query parameters of normal searches it also supports a parameter called 'triggerState' which expects a datetime value in the format `YYYY-MM-DDThh:mm:ssTZD`, for example `2020-09-15T05:05:42Z`. The polling endpoint will do a search that includes both the standard 'q' query parameter and the query 'updated>triggerState'.

### Location header in response

The search response will contain a [`location`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/location) header which tells the consumer what the next request should be. This will be different depending on whether there are more records available matching the current query or not. If there are no more records then the triggerState will be the current DateTime. If there are more records then the triggerState will be the same as in the request and the 'start' parameter will be set to the beginning of the next page of results.

### retry-after header in response

In addition to the location header the response will also contain a [`retry-after`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) header, this tells the consumer how long to wait until sending the next request. If there is a subsequent page in the current result set then retry-after will be 0, if not it will be 15 seconds.

### Location header examples

#### Original request

```bash
https://MyServer/ServiceAPI/Polling?q=all&trimType=Record&resultsOnly=true&pageSize=30&triggerState=2020-09-15T05:05:42Z
```

#### Loction header if no more matching records

```bash
https://MyServer/ServiceAPI/Polling?q=all&trimType=Record&resultsOnly=true&pageSize=2&triggerState=2020-09-16T02:25:44Z&start=0
```

#### Location header if there are more matching Records

```bash
https://MyServer/ServiceAPI/Polling?q=all&trimType=Record&resultsOnly=true&pageSize=2&triggerState=2020-09-15T05:05:42Z&start=30`
```
