---
layout: post
title: Composing a search string
categories: []
tags: serviceapi dotnet-sdk
status: publish
type: post
published: true
meta: {}
---

## Overview

Search strings are required for searching in the web service and can be convenient in the .Net SDK, there are a few rules on how to compose these strings but if I need to compose a complex search to get baked into an application I like to use the Content Manager client to assist me.

### Composing the search string

To compose the search string:

1. construct the search in the Content Manager client
1. go to 'refine search'
1. select Editor > String
1. if there is a chance the string will be used in different languages, or that the search caption may get changed then replace the clause names with the internal names from the [SDK documentation](https://content-manager-sdk.github.io/Community/).

#### Example

![](/images/string_search.png)

### Look at me!

For those who enjoy the sound of my voice you can watch me compose a search string here.

<iframe src="https://player.vimeo.com/video/415714418?app_id=122963&amp;wmode=opaque" width="640" height="400" frameborder="0" title="Power BI" allow="autoplay; fullscreen" allowfullscreen=""></iframe>