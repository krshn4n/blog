---
layout: post
title: Logic Apps (and Flow) Custom Connector
categories: []
tags: serviceapi logic-apps
status: publish
type: post
published: true
---

Connect to Content Manager from Microsoft Logic Apps (or Flow) using a custom connector. A new, very simple, [OpenAPI definition](https://github.com/content-manager-sdk/Community/tree/master/Samples/ServiceAPI/Connectors/Logic%20Apps) is available to assist in creating the Logic Apps custom connector.

<iframe src="https://player.vimeo.com/video/359033339?app_id=122963&amp;wmode=opaque" width="640" height="400" frameborder="0" title="Power BI" allow="autoplay; fullscreen" allowfullscreen=""></iframe>

## The past

Over time we have had a few inquiries about an OpenAPI definition for the ServiceAPI, maybe prompted by the fact that we include the swagger UI. While the swagger ui is based on an OpenAPI/swagger definition we have never released this publicly, largely because this definition file is auto-generated from the ServiceAPI and is therefore very large and complex. My observation of client code generated from this specification is that it is close to unusable.

## The future

If an OpenAPI definition file will be useful for things like creating Logic Apps custom connectors a good approach may be to create a tool to generate an OpenAPI definition file on demand, including only the portions of the ServiceAPI required by the customer. This allows us to avoid both the size and complexity problems. Any feedback you have will help us determine what direction to take this in.
