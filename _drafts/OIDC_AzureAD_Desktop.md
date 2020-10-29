---
layout: post
title: Configuring OpenId Connect for Azure AD in the Content Manager desktop client
categories: []
tags: azure-ad
status: publish
type: post
published: true
meta: {}
---

## Overview

OpenID Connect may be used to authenticate with the Content Manager desktop client, this document details how to configure this.

### Create the Azure AD Application

To create the Azure AD application:

1. From with portal.azure.com go to Azure AD.
1. Go to App Registrations and select New Registration.
1. Enter a name.
1. Select 'Public native client' in the Redirect URI
1. Enter this redirect Uri 'urn:ietf:wg:oauth:2.0:oob'


#### Example

![](/images/azuread_app_desktop_1.png)

### Configure permissions

From API Permissions add the following delegated Microsoft Graph permissions:
 * email
 * offline_access
 * openid
 * profile
 * User.Read
 
 Select 'Grant Admin Access' to grant access to all permissions.

#### Example

![](/images/azuread_app_desktop_2.png)

### Configure authentication in Content Manager Enterprise Studio

To use the Azure AD app created above open Content Manager Enterprise Studio and:
1. from the database choose Authentication > OpenID
1. The OpenID Issuer URL is taken from the Azure App - Ovrview > Endpoints > OpenID Connect metadata document
1. The Client Id is taken from the Azure App - Ovrview > Application ID
1. Client secret should be empty
1. Client app scope should contain 'openid email offline_access'



#### Example

![](/images/azuread_app_desktop_3.png)