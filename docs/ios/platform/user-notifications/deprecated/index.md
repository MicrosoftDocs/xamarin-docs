---
title: "Deprecated Notification Technologies in Xamarin.iOS"
description: "This document describes iOS notification technologies that have been deprecated in favor of the User Notifications framework, introduced in iOS 10."
ms.service: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2016
no-loc: [Objective-C]
---

# Deprecated Notification Technologies in Xamarin.iOS

This section shows how to implement local and push notifications
  in Xamarin.iOS. It will explain the various UI elements of an
  iOS notification and discuss the API's involved with creating
  and displaying a notification.

> [!IMPORTANT]
> The information in this section pertains to iOS 9 and prior, it has been left here to support older iOS versions. For iOS 10 and later, please see the [User Notification Framework guide](~/ios/platform/user-notifications/index.md) for supporting both Local and Remote Notification on an iOS device.

## Sections

<a name="Local Notifications In iOS"></a>

## [Local Notifications in iOS](local-notifications-in-ios.md)

This section will discuss how to implement local notifications in Xamarin.iOS. It will
    explain the various
    UI elements of an iOS notification and discuss the API's involved with creating and displaying a notification.

<a name="Local Notifications Walkthrough"></a>

## [Walkthrough - Using Local Notifications in Xamarin.iOS](local-notifications-in-ios-walkthrough.md)

In this section we'll walk through how to use local notifications in a
    Xamarin.iOS application. It will
    demonstrate the basics of creating and publishing a notification that will pop
    up an alert when received by the app.

<a name="Remote Notifications In iOS"></a>

## [Remote Notifications in iOS](remote-notifications-in-ios.md)

This section will cover push notifications in iOS. It introduces the Apple Push
    Notifications Gateway Service (APNS) and the role it plays in publishing notifications to iOS applications. It will explain how to create the security certificates
    necessary to enable push notifications and discuss. Finally this section will discuss some of the housekeeping tasks that application servers must perform to keep track of the client mobile devices.

## Related Links

- [Notifications (sample)](/samples/xamarin/ios-samples/notifications)