---
title: "Using a ContentProvider"
ms.service: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
description: Learn how to use and access a ContentProvider with Xamarin.Android, allowing for the access of data exposed by other applications.
---

# Using a ContentProvider with Xamarin.Android

CursorAdapters can also be used to display data from a ContentProvider.
ContentProviders allow you to access data exposed by other applications
(including Android system data like contacts, media and calendar
information).

The preferred way to access a ContentProvider is with a CursorLoader using 
the LoaderManager. LoaderManager was introduced in Android 3.0
(API Level 11, Honeycomb) to move blocking tasks off the main thread,
and using a CursorLoader allows the data to be loaded in a thread
before being bound to a ListView for display.

Refer to
[Intro to ContentProviders](~/android/platform/content-providers/index.md)
for more information.
