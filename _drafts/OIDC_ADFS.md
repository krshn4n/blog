---
layout: post
title: Configuring OpenId Connect for Azure AD
categories: []
tags: adfs serviceapi authentication web-client
status: publish
type: post
published: true
meta: {}
---

## Overview

As of CM 10 the CM web applications (ServiceAPI, WebDrawer and Web Client) have an OpenId Connect authentication provider built in. This document describes creating an AD Fs application and configuring the CM web application.

### Create the AD FS  Application

To create the AD FS application:

1. create a new Application Group
1. select 'server application accessing a web application'
1. Enter a name.
1. press Next
1. take note of the client identifier
1. add a Redirect URI. The redirect URI must be all in lowercase and be the URL of the Content Manager web site with the suffix '/serviceapi/auth/openid.  For example: https://myserver/contentmanager/serviceapi/auth/openid.
1. press Next
1. Generate a shared secret and take note of this secret
1. press next
1. add an identifier, for example: https://MyServer/contentmanager/
1. press next
1. choose an access control policy (for example: 'Give access to everyone
1. In 'Configure Application permissions select email, openid, and profile
1. Complete the Application Group

#### Examples

![](/images/oidc_adfs_1.png)
![](/images/oidc_adfs_2.png)
![](/images/oidc_adfs_3.png)
![](/images/oidc_adfs_4.png)


### Add the settings to the Web Client

To configure the Web Client  edit the hprmServiceApi.config file and add (or edit) the authentication element to look similar to the example below.
 
 * Client Id and secret come from the previous section
 * issuerUri is found in the AD FS console - Endpoints, in the OpenID Connect section

#### Example
```xml
  <authentication allowAnonymous="false" slidingSessionMinutes="30">
    <openIdConnect>
      <add 
		name="openid" 
		clientID="CLIENT_ID" 
		clientSecret="SECRET" 
		issuerURI="https://MyServer/adfs/.well-known/openid-configuration"/>
    </openIdConnect>
  </authentication>
```

### Configure AD FS for Office Integration access

The office integration requires an access token to allow it ro authenticate with the Web Client, this can be configured in AD FS:

1. go the Application group configured above
1. use 'Add Application' to add a native application
1. preserve Client Id for later use
1. comnplete the native application
1. edit the Web API application and in Client Permissions add the new client application selecting the scopes email, openid and profile
1. in Issuance Transform Rules add a new Rule 
1. select 'Send LDAP Attributes 
1. select Next
1. choose Active Directory as the attribute store
1. mape these two claims, 
	* Display Name - Name
	* User-Principal-Name - UPN
1. Finish

### Add office integration the settings to the Web Client

The office integration authentication settings are stored in the file ADFS\config.xml in the web client install directory:
 * clientAuthority - the URL to your AD FS server
 * clientResourceUri - the relying party identifier from the AD Fs Web API
 * clientID - the Client Id from the AD FS native application
 * The Redirect URI from the AD FS native application


```xml
<adfsClient>
	<clientAuthority>https://acme.com/adfs</clientAuthority>
	<clientResourceUri>https://acme.com/contentmanager/</clientResourceUri>
	<clientID>ab762716-544d-4aeb-a526-687b73838a34</clientID>
	<clientReturnUri>https://btpvm2237.myexch19.com/contentmanager/</clientReturnUri>
</adfsClient>
```