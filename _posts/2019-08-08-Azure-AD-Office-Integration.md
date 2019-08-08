---
layout: post
title: Authenticating in the web client and Office integration with Azure AD
categories: []
tags: azure-ad web-client office-integration
status: publish
type: post
published: true
---

Configuring the Web Client and Office integration to use Azure AD for authentication is a multi-step process.  Here I run through a simple scenario to get this working, first in a video and following some sample config.



<iframe src="https://player.vimeo.com/video/352605693?app_id=122963&amp;wmode=opaque" width="640" height="400" frameborder="0" title="Power BI" allow="autoplay; fullscreen" allowfullscreen=""></iframe>

## web.config
First add the below to the configSections element.

```.xml
    <section name="system.identityModel" type="System.IdentityModel.Configuration.SystemIdentityModelSection, System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" />
    <section name="system.identityModel.services" type="System.IdentityModel.Services.Configuration.SystemIdentityModelServicesSection, System.IdentityModel.Services, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" />
```

Add this element to appSettings. Replace the value with the federation metadata URL from your Azure AD App.

```.xml
    <add key="ida:FederationMetadataLocation" value="https://login.microsoftonline.com/xxx-xxx-xxx-xxx-xxx/federationmetadata/2007-06/federationmetadata.xml" />
```

Replace the authentication element with this:

```.xml
    <authorization>
      <deny users="?" />
    </authorization>
    <authentication mode="None" />
```

Near the end of the web.config (just below the end configuration tag) include this:

```.xml

  <system.identityModel>
    <identityConfiguration>
      <audienceUris>
        <add value="[APP IF FROM WEB APP REGISTRATION]" />
        <add value="[APPLICATION ID URI FROM 'EXPOSE AND API' IN WEB APP REGISTRATION]"/>
      </audienceUris>
      <securityTokenHandlers>
        <add type="System.IdentityModel.Services.Tokens.MachineKeySessionSecurityTokenHandler, System.IdentityModel.Services, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        <remove type="System.IdentityModel.Tokens.SessionSecurityTokenHandler, System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </securityTokenHandlers>
      <certificateValidation certificateValidationMode="None" />

      <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/[TENANT ID]/">
          <keys>

          </keys>
          <validIssuers>
            <add name="https://sts.windows.net/[TENANT ID]/" />
          </validIssuers>
        </authority>
      </issuerNameRegistry>
    </identityConfiguration>
  </system.identityModel>
  <system.identityModel.services>
    <federationConfiguration>
      <cookieHandler requireSsl="true" />
      <wsFederation passiveRedirectEnabled="true" issuer="https://login.windows.net/[YOUR DOMAIN]/wsfed" realm="[APPLICATION ID URI FROM 'EXPOSE AND API' IN WEB APP REGISTRATION]" requireHttps="true" />
    </federationConfiguration>
  </system.identityModel.services>
  <location path="adfs">
    <system.web>
      <authorization>
        <allow users="?" />
      </authorization>
    </system.web>
  </location>

```

## hprmServiceAPI.config

Add the authentication element:

```.xml
  <authentication allowAnonymous="false">
    <activeDirectory>
      <add name="adfs" audience="[APPLICATION ID URI FROM 'EXPOSE AND API' IN WEB APP REGISTRATION]" metadataEndpoint="[FEDERATION METADATA DOCUMENT]"/>
    </activeDirectory>
  </authentication>
```


Add useADFS="true" to the setup element.

```.xml
 <setup  databaseId="M1" useADFS="true" 
```

## config.xml
Create the config.xml file in the ADFS folder under your web client install folder.

```.xml
<adfsClient>
<clientAuthority>https://login.windows.net/[YOUR DOMAIN]</clientAuthority>
<clientResourceUri>[APPLICATION ID URI FROM 'EXPOSE AND API' IN WEB APP REGISTRATION]</clientResourceUri>
<clientID>[APPLICATION ID IN NATIVE APP REGISTRATION]</clientID>
<clientReturnUri>[replyUrlsWithType url FROM MANIFEST IN NATIVE APP REGISTRATION</clientReturnUri>
</adfsClient>
```