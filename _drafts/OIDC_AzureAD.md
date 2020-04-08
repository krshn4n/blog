---
layout: post
title: Configuring OpenId Connect for Azure AD
categories: []
tags: dotnet-sdk
status: publish
type: post
published: true
meta: {}
---

## Overview

As of CM 10 the CM web applications (ServiceAPI, WebDrawer and Web Client) have an OpenId Connect authentication provider built in. This document describes creating an Azure Ad application and configuring the CM web application.

### Create the Azure AD Application

To create the Azure AD application:

1.  Go to App Registrations and select New Registration
2.  Enter a name
3.  Under Redirect URI leave Web selected
4.  The value in the Redirect URI is important, it must be lowercase and it must be the URL to your application (e.g. https://mydomain.com/CMSWebDrawer) followed by the path to the authentication provider (for example /auth/openid). The /auth/ component is fixed but the 'openid' is the name you will supply in hptrim.config later and so can be any string, as long as it matches the value in hptrim.config.

#### Example

![](/images/azuread_app_1.png)
