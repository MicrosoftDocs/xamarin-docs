---
title: "Data and Cloud Services in Xamarin.iOS Apps"
description: "This document links to guides that describe how to work with local data, iCloud, and CloudKit in a Xamarin.iOS app."
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
---

# Data and Cloud Services in Xamarin.iOS Apps

##  [Data Access](~/ios/data-cloud/data/index.md)

Most applications have some requirement to save data on the device locally. Unless the amount of data is trivially small, this usually requires a database and a data layer in the application to manage database access. iOS has the Sqlite database engine “built in” and access to the data is simplified by Xamarin’s platform which comes with the SQLite Data Provider.

##  [iCloud](~/ios/data-cloud/introduction-to-icloud.md)

The iCloud storage API in iOS 5 allows applications to save user documents and application-specific data to a central location and access those items from all the user's devices.

##  [CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

The CloudKit framework streamlines the development of applications that access iCloud. This includes the retrieval of
application data and asset rights as well as being able to securely store application information. This kit gives users a layer
of anonymity by allowing access to applications with their iCloud IDs without sharing personal information.