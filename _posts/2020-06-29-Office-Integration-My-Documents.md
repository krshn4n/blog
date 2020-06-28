---
layout: post
title: Office Integration 'My Documents' folder
categories: []
tags: office-integration
status: publish
type: post
published: true
meta: {}
---

The CM Office Integration uses a local folder in which to store its working documents. Prior to 94 this folder was always found within the Windows 'My Documents' folder. Due to conflicts with OneDrive this was changed in 94 and it can now be set globally or overridden by each user.

### Global Setting

The default 'My Documents' may be set in the registry using the key 'OfficeCheckouts' within the path `Computer\HKEY_CURRENT_USER\Software\Micro Focus\Content Manager\DataPaths`. This may, of course, contain variable such as %AppData%.

### Global Setting (for Web Client)

When connecting to CM via the Web Client, the registry setting above is not used and instead the 'My Documents' will always be located in the path `%AppData%\Micro Focus\Content Manager\[DatabaseID]\OfficeCheckouts`.

### Overriding the default

The end user has the ability to override the default using the CM Options dialog, any changes made will be stored in the CM Office Integration preferences file found in `%AppData%\Micro Focus\Content Manager\OfficeIntegration`.

### If nothing is set

If the registry does not contain the 'OfficeCheckouts' key and the user has not explicitly set the path then the Office Integration 'My Documents' path will be located under Windows 'My Documents' folder as in previous versions.
