---
title: "watchOS platform features"
description: "This document links to various guides that describe watchOS platform features such as Apple Pay, notifications, complications, proactive suggestions, workout apps, and more."
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
---
# watchOS platform features

## [Introduction to watchOS 5](introduction-to-watchos5/index.md)

This document provides a high-level overview of new and updated features in
watchOS 5 that are available for use when building watchOS apps with
Xamarin.

## [Introduction to watchOS 4](introduction-to-watchos4.md)

This document provides a high-level overview of features added and 
updated in watchOS 4.

## [Introduction to watchOS 3](introduction-to-watchos3/index.md)

This article describes new and updated APIs in watchOS 3.

## [Apple Pay enhancements](~/ios/watchos/platform/apple-pay.md)

In watchOS 3, the PassKit framework has been expanded to allow support
for secure, in-app payments (of both physical goods and services) for the
apps running on the Apple Watch.

## [Background tasks](~/ios/watchos/platform/background-tasks.md)

watchOS 3 introduces several background tasks that an app can use to
update its information ensuring that it has the content the user needs
before they open it.

## [Notifications](notifications.md)

Learn how to provide custom notification handling in your watch app.

## [Complications](complications.md)

Add complication support to display up-to-date data on the watch face.

## [Proactive suggestions](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 allows the app to proactively present information to the user
within given contexts. To support this feature, the
[NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)
now includes the `MapItem` property that lets the app provide location
information for later use by other apps.

## [Quick interaction techniques](~/ios/watchos/platform/quick-interaction-techniques.md)

Providing quick user interactions are essential to creating compelling
Apple Watch apps and Complications. New to watchOS 3, Apple has added
support for Gesture Recognizers, access to the Digital Crown and new User
Notification and navigation techniques. This, along with added support
for both SceneKit and SpriteKit, allow the developer to easily create
rich, glanceable interfaces that are both quick and responsive.

## [Workout app enhancements](~/ios/watchos/platform/workout-apps.md)

New to watchOS 3, workout related apps have the ability to run in the
background on the Apple Watch. To enable this feature (and gain access to
HealthKit data), the app must include the `WKBackgroundModes` key in the
`Info.plist` file with the value `workout-processing`.
