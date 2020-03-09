---
layout: post
title: Unable to sign out from Web Client
categories: []
tags: web-client
status: publish
type: post
published: true
---

I stumbled on an odd problem the other day. A CM web client instance authenticating use WS-Fed (ADFS). The user could sign-in without problem but when they attempted to sign-out they got the response below.
![](images/auth_response_xml.png)

The problem is that the Continue parameter in the sign out URL (see below) was not firing. The process to sign out is a two step on, first call the logout service to clear the web service session and then call the WS-Fed logout via the Continue parameter.

```
https://myserver.com/ContentManager/ServiceApi/auth/logout?Continue=/ContentManager/ServiceApi/auth/adlogout
```

#### The Solution

The solution to this problem was in the hprmServiceapi.config file. The serviceFeatures property must contain Html for the Continue parameter to function. For some reason Html had been removed from serviceFeatures.
