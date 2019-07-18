---
layout: post
title: Update to site
categories: []
tags: serviceapi
status: draft
type: post
published: false
---

When posting a date to an Additional Field in the web service the date returned in the response is incorrect, even though the date set in CM is correct.  The problem is caused by a bug in handling time zone offsets.

### Example with C# client

The code below updates both the Date Due and an Additional Field called 'Date of Issue'.  

```C#
DateTime dateTime = DateTime.Parse("2019-07-18T04:03:19Z");

Record record = new Record();
record.Uri = 9000000221;

record.SetCustomField("DateOfIssue", dateTime);

record.DateDue = new TrimDateTime() { DateTime = dateTime, IsClear = false };
var response = trimClient.Post<RecordsResponse>(record);


foreach (var rec in response.Results)
{
	Console.WriteLine(rec.DateDue.DateTime.Value.ToUniversalTime());

	Console.WriteLine(((TrimDateTime)rec.Fields["DateOfIssue"]).DateTime.Value.ToUniversalTime());
}
```
