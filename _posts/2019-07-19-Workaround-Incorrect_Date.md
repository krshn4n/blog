---
layout: post
title: Workaround for incorrect date returned in Additional Fields
categories: []
tags: serviceapi
status: publish
type: post
published: true
---

When posting a date to an Additional Field in the web service the date returned in the response is incorrect, even though the date set in CM is correct.  The problem is caused by a bug in handling time zone offsets.  The scenarios in this post assume that:
 * all actors (web service, client, WGS) are using the same time-zone settings, and
 * dates are always sent as UTC (not local) dates.

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

The response from this code looks like this, even though the dates should be the same.

![alt text](https://raw.githubusercontent.com/content-manager-sdk/blog/master/images/date_different.PNG "dates different")

But the dates in the cM client are correct, I am at UTC+10 so 2:03PM is correct for 4:03 AM UTC.

![alt text](https://github.com/content-manager-sdk/blog/raw/master/images/dates_same.PNG "dates in CM")

### Example with JSON Http Post
Posting the JSON below is the equivalent to the C# code above

```JSON5
{
	"Uri":9000000221,
	"RecordDateDue":"2019-07-18T04:03:19Z",	"Fields": {"DateOfIssue": "2019-07-18T04:03:19Z"}
}
```

The response looks like this:

```JSON5
{
    "Results": [
        {
            "TrimType": "Record",
            "RecordDateDue": {
                "IsClear": false,
                "IsTimeClear": false,
                "DateTime": "2019-07-18T04:03:19.0000000Z"
            },
            "Uri": 9000000221,
            "Fields": {
                "DateOfIssue": {
                    "__type": "HP.HPTRIM.ServiceModel.TrimDateTime, HP.HPTRIM.ServiceAPI.Model",
                    "IsClear": false,
                    "IsTimeClear": false,
                    "DateTime": "2019-07-18T14:03:19.0000000Z"
                }
            }
        }
    ],
...
}

```

### A workaround
A fix to this bug will be released soon, in the meantime here are two alternate work-arounds.

#### Reverse the time-zone offset
The underlying error is an incorrect application of the time-zone offset, this can be fixed by subtracting the offset from the returned date.

#### Use the StringValue, not the DateTime property
All properties return both the native value (e.g. DateTime) and a string representation of that value.  To return a sting in addition to the native value specify the PropertyValue and StringDisplayType properties on the Record object.  The StringValue will contain the correct DateTime value in the users local timezone.

#### Sample code

```C#
DateTime dateTime = DateTime.Parse("2019-07-18T04:03:19Z");

Record record = new Record();
record.Uri = 9000000221;
record.PropertyValue = PropertyType.Both;
record.StringDisplayType = StringDisplayType.WebService;

record.SetCustomField("DateOfIssue", dateTime);

record.DateDue = new TrimDateTime() { DateTime = dateTime };
var response = trimClient.Post<RecordsResponse>(record);


foreach (var rec in response.Results)
{
	// stock date is correct
	Console.WriteLine(rec.DateDue.DateTime.Value.ToUniversalTime());

	// subtract offset from Additional Field Date
	TimeSpan offset = TimeSpan.FromMinutes(TimeZoneInfo.Local.BaseUtcOffset.TotalMinutes);
	DateTime correctedDate = ((TrimDateTime)rec.Fields["DateOfIssue"]).DateTime.Value.Subtract(offset);
	Console.WriteLine(correctedDate.ToUniversalTime());

	// get Date/Time from string value
	DateTime dateOfIssue = DateTime.Parse(rec.Fields["DateOfIssue"].StringValue);

	Console.WriteLine(dateOfIssue.ToUniversalTime());

}
```
