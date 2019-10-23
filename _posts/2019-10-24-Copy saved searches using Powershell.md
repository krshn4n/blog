---
layout: post
title: Copy your saved searches from database to database using Powershell
categories: []
tags: dotnet-sdk
status: publish
type: post
published: true
---

Maybe you have a testing or POC database in which you have a bunch of Saved Searches and you want to export these and import them to your production database. These Powershell scripts will export and import the search related properties, not the ACL of course.

### Export

The code below:

- connects to the database N1 on the local machine
- does a search for a saved search with the URI 9000000010
- writes the XML representation of that saved search to a file

```PS1
$assemblyPath = "d:\trunk\x64\Debug\HP.HPTRIM.SDK.dll"

Add-Type -Path $assemblyPath

$database = New-Object HP.HPTRIM.SDK.Database
$database.Id = "N1"
$database.WorkgroupServerName = "local"
$database.Connect()

$searchSearch = New-Object HP.HPTRIM.SDK.TrimMainObjectSearch($database, [HP.HPTRIM.SDK.BaseObjectTypes]::SavedSearch)
$searchSearch.SetSearchString("unkUri:9000000010")

foreach ($savedSearch in $searchSearch ) {

   $outputPath = "c:\junk\searches\" + $savedSearch.Name + ".xml"

   $savedSearch.GetSearch().SearchAsXml | out-file -append $outputPath

}
```

### Import

This code:

- connects to the database N2 on the local machine
- reads all the files in the folder
- creates a TrimMainObjectSearch and sets the SearchAsXml property from the saved XML
- creates a Saved Search from the TrimMainObjectSearch

```PS1
$assemblyPath = "d:\trunk\x64\Debug\HP.HPTRIM.SDK.dll"

Add-Type -Path $assemblyPath

$database = New-Object HP.HPTRIM.SDK.Database
$database.Id = "N2"
$database.WorkgroupServerName = "local"
$database.Connect()

$files = @(Get-ChildItem c:\junk\searches\*.xml)
foreach ($file in $files) {

   $newSearch = New-Object HP.HPTRIM.SDK.TrimMainObjectSearch($database, [HP.HPTRIM.SDK.BaseObjectTypes]::Record)
   $newSearch.SearchAsXML = [System.IO.File]::ReadAllText($file.FullName)


   $savedSearch = New-Object HP.HPTRIM.SDK.SavedSearch($newSearch)
   $savedSearch.Name = $file.BaseName
   $savedSearch.Save()
}

```
