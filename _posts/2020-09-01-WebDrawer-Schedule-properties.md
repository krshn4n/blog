---
layout: post
title: Displaying the properties of a Schedule in WebDrawer
categories: []
tags: webdrawer
status: publish
type: post
published: true
meta: {}
---

At some point since CM 8.3 the method used to determine which properties to display on a page was changed to allow the hptrim.config file to determine which properties are displayed. One omission in this change was the default hptrim.config was not updated to specify which properties to display for a retention schedule. This post describes how to remedy that omission.

### What is there now (pre CM 10)

If you look in hptrim.config you will see an element inside the routeDefaults element that looks like this.

```xml
<add
	name="Schedule"
	model="ScheduleFind"
	template="WDScheduleDetails"
	properties="NameString,ScheduleName,ScheduleNotes,ScheduleSource,ScheduleInUse,ScheduleIsAgencySpecific,ScheduleTriggerIsForDestroy,ScheduleActiveFrom,ScheduleActiveTo,ScheduleActiveDescription"
/>
```

### The solution

To include the properties for display in the Schedule page, first add a new element inside the 'customPropertySets' element, it should look like this:

```xml
<add
  name="ScheduleIdentification"
  properties="NameString,ScheduleName,ScheduleNotes,ScheduleSource,ScheduleInUse,ScheduleIsAgencySpecific,ScheduleTriggerIsForDestroy,ScheduleActiveFrom,ScheduleActiveTo,ScheduleActiveDescription"
/>
```

Next go back to the Schedule element inside 'routeDefaults' and modify it to look like this:

```xml
<add
  name="Schedule"
  model="ScheduleFind"
  template="WDScheduleDetails"
  propertySets="Identification"
/>
```

### What is going on here

The code that displays properties on a page looks for display properties in the customPropertySets. The properties listed in each property set are displayed in a seperate section on the page, in the case of Schedule
