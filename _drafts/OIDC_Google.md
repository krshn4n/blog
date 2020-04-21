---
layout: post
title: Configuring OpenId Connect for Google
categories: []
tags: google serviceapi authentication web-client webdrawer
status: publish
type: post
published: true
meta: {}
---

## Overview

As of CM 10 the CM web applications (ServiceAPI, WebDrawer and Web Client) have an OpenId Connect authentication provider built in. This document describes creating Google credentials and configuring the CM web application.

### Create the Google Credentials

To create the Google credentials:

1.  Go to https://console.developers.google.com/,
2.  Select Credentials > OAuth Client ID,
3.  Set Application type to 'Web Application'.
4.  Add your domain in the Authorized JavaScript origins
5.  The value in Authorized redirect URIs is important, it must be lowercase and it must be the URL to your application (e.g. https://mydomain.com/CMWebDrawer) followed by the path to the authentication provider (for example /auth/openid). The /auth/ component is fixed but the 'openid' is the name you will supply in hptrim.config later and so can be any string, as long as it matches the value in hptrim.config.
6.  On saving the Client ID and Client Secret will be displayed, preserve them for later use.

#### Example

![](/images/google_credentials.png)

### Configure authentication in hptrim.config

To use the Google credentials created above edit hptrim.config (hprmServiceAPI.config in the Web Client) so that it has am authentication similar to the one below.

1.  The name must match the last segment of the Redirect URI path.
2.  Client ID is the one preserved earlier.
3.  The secret is the one preserved earlier.
4.  The issuerURI is: https://accounts.google.com

#### Example config

```xml
	<authentication allowAnonymous="false" slidingSessionMinutes="2">
		<openIdConnect>
			<add
				name="openid"
				clientID="741419278958-cdn3tbbbomrrbk71le811cr0nnjtpilb.apps.googleusercontent.com"
				clientSecret="jl-BiX298Z1khwNRO5L0W15f"
				issuerURI="https://accounts.google.com"
			/>
		</openIdConnect>
	</authentication>
```

### Enable redirect

The web client will not re-direct the authentication endpoint unless the Html feature is enabled in hprmServiceAPI.config. To do this:

1.  edit hprmServiceAPI.config
2.  find the property named 'serviceFeatures'
3.  Add the feature Html

### Logout

For WebDrawer the logout link is configured in the uiSettings. It should contain '~/auth/logout'. In the Web Client a logout link will be displayed automatically when OpenId Connect authentication is enabled.

#### Example

```xml
  <uiSettings
    logoutLink="~/auth/logout"
	...
  />
```
