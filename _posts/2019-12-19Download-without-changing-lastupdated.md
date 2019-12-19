---
layout: post
title: Copy your saved searches from database to database using Powershell
categories: []
tags: dotnet-sdk serviceapi
status: publish
type: post
published: true
---

The default behaviour in Content Manager is to update the data LastUpdated when getting an electronic document from a Record.  This can be problematic, particularly in cases where you want to get the document every time the Record changes.  Thankfully this update can be override.

### .Net SDK

The code below:

- uses ExtractDocument to get the document
- sets SuppressLastAction to true to bypass setting the Date Last Updated action,
- optionally sets LofAsViewed so that the audit log contains a 'Viewed' entry rather than an 'Extracted' entry.

```CS
				ExtractDocument extractDocument = record.GetExtractDocument();
				extractDocument.SuppressLastAction = true;
				extractDocument.FileName = "rec_1_1.docx";
				extractDocument.LogAsViewed = true;
				extractDocument.DoExtract("c:\\junk", true, false, null);
```

### ServiceAPI

To download a document using the ServiceAPI using the same options demonstrated above using C# use a URL similar to the following:

```
/ServiceAPI/Record/9000000001/File/document?SuppressLastAction=true
```
