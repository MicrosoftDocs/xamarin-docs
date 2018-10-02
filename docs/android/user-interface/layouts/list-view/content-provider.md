---
title: "Using a ContentProvider"
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# Using a ContentProvider

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

