---
title: "Intro to ContentProviders"
description: "The Android operating system uses content providers to facilitate access to shared data such as media files, contacts and calendar information. This article introduces the ContentProvider class, and provides two examples of how to use it."
ms.service: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
---

# Intro to ContentProviders

_The Android operating system uses content providers to facilitate access to shared data such as media files, contacts and calendar information. This article introduces the ContentProvider class, and provides two examples of how to use it._

## Content Providers Overview

A *ContentProvider* encapsulates a data repository and provides an API to access
it. The provider exists as part of an Android application that usually also
provides a UI for displaying/managing the data. The key benefit of using a content
provider is enabling other applications to easily access the encapsulated 
data using a provider client object (called a *ContentResolver*). Together, a content
provider and content resolver offer a consistent inter-application API for data
access that is simple to build and consume. Any application can choose to use
`ContentProviders` to manage data internally and also to expose it to other applications.

A `ContentProvider` is also required for your application to provide
custom search suggestions, or if you want to provide the ability to
copy complex data from your application to paste into other
applications. This document shows how to access and build
`ContentProviders` with Xamarin.Android.

The structure of this section is as follows:

- **How it works** &ndash; An overview of what the `ContentProvider` is
designed for and how it works.

- **Consuming a Content Provider** &ndash; An example accessing the
Contacts list.

- **Using ContentProvider to share data** &ndash; Writing and
consuming a `ContentProvider` in the same application.

`ContentProviders` and the cursors that operate on their data are often
used to populate ListViews. Refer to the
[ListViews and Adapters guide](~/android/user-interface/layouts/list-view/index.md)
for more information on how to use those classes.

`ContentProviders` exposed by Android (or other applications) are an
easy way to include data from other sources in your application. They
allow you to access and present data such as the Contacts list, photos
or calendar events from within your application, and let the user
interact with that data.

Custom `ContentProviders` are a convenient way to package your data for
use inside your own app, or for use by other applications (including
special uses like custom search and copy/paste).

The topics in this section provide some simple examples of consuming
and writing `ContentProvider` code.

## Related Links

- [ContactsAdapter Demo (sample)](/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (sample)](/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [Content Providers Developers Guide](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider Class Reference](xref:Android.Content.ContentProvider)
- [ContentResolver Class Reference](xref:Android.Content.ContentResolver)
- [ListView Class Reference](xref:Android.Widget.ListView)
- [CursorAdapter Class Reference](xref:Android.Widget.CursorAdapter)
- [UriMatcher Class Reference](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract Class Reference](xref:Android.Provider.ContactsContract)