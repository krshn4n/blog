---
layout: post
title: Using TrimDateTime from a web service
categories: []
tags: dotnet-sdk
status: publish
type: post
published: true
---

When writing a web service that uses the CM SDK you need to take into account the fact that different users may be in different time zones.  The SDK date/time object, TrimDateTime operates, buy default, in the same time zone as the operating system.  To ensure you set dates correctly you need to convert it to operate in the same time zone as the user.  The [sample code](https://github.com/content-manager-sdk/Community/tree/master/Samples/SDK/CSharp/DateTimeConverter/) demonstrates how to do this, for more detail have a look at this video.



<iframe src="https://player.vimeo.com/video/352171193?app_id=122963&amp;wmode=opaque" width="640" height="400" frameborder="0" title="Power BI" allow="autoplay; fullscreen" allowfullscreen=""></iframe>
