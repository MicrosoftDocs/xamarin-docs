---
title: "Introduction to watchOS 3"
description: "This article introduces all of the new and modified APIs and features available in watchOS 3 for Xamarin developers."
ms.service: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2017
no-loc: [Objective-C]
---

# Introduction to watchOS 3

_This article introduces all of the new and modified APIs and features available in watchOS 3 for Xamarin developers._

This document will cover the following topics:

- [What's New in watchOS 3](#Whats-New-in-watchOS-3)
  - [Apple Pay Enhancements](#Apple-Pay-Enhancements) adds support for in-app payments on the Apple Watch.
  - [Background Tasks](#Background-Tasks) give the app the ability to update its information in the background so it is ready when the user needs it.
  - [Complications Enhancements](#Complications-Enhancements) have been made for watchOS 3 that provide new features for the apps.
  - [Newly Available Frameworks](#Newly-Available-Frameworks) have exposed for the watchOS apps.
  - [Proactive Suggestions](#Proactive-Suggestions) allows the app to proactively show information to the user.
  - Several [Security and Privacy Enhancements](#Security-and-Privacy-Enhancements) have been made to watchOS 3.
  - [Snapshots and Dock](#Snapshots-and-Dock) provide the user with quick access to the app watchOS apps.
  - [User Notifications](#User-Notifications) provides both local and remote notifications to the user.
  - Several [Watch Connectivity Framework Enhancements](#Watch-Connectivity-Framework-Enhancements) have been made in watchOS 3.
  - Several [WatchKit Framework Enhancements](#WatchKit-Framework-Enhancements) have been made in watchOS 3.
  - [Workout App Enhancements](#Workout-App-Enhancements) gives new abilities to the workout related Apple Watch apps.
- [Additional Framework Changes](#Additional-Framework-Changes) have been made throughout watchOS 3.
- [Deprecated APIs](#Deprecated-APIs) in watchOS 3.

<a name="Whats-New-in-watchOS-3"></a>

## What's New in watchOS 3

Apple has added several new APIs and services in watchOS 3 along with many enhancements to existing features, including:

<a name="Apple-Pay-Enhancements"></a>

## Apple Pay Enhancements

In watchOS 3, the PassKit framework has been expanded to allow support for secure, in-app payments (of both physical goods and services) for the apps running on the Apple Watch.

Use the new [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) and [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) classes to present and respond to an interface where the user can authorize payment requests.

To find out more, please see our [Apple Pay Enhancements](~/ios/watchos/platform/apple-pay.md) guide.

<a name="Background-Tasks"></a>

## Background Tasks

watchOS 3 introduces several background tasks that an app can use to update its information ensuring that it has the content the user needs before they open it.

The following new background tasks are available:

- **Background App Refresh** - The [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) task allows the app to update its state in the background. Typically this will include another task such as downloading new content from the internet using a [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Background Snapshot Refresh** - The [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) task allows the app to update both its content and UI before the system takes a snapshot that will be used to populate the Dock.
- **Background Watch Connectivity** - The [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) task is started for the app when it receives background data from the paired iPhone.
- **Background URL Session** - The [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) task is started for the app when a background transfer requires authorization or completes (successfully or in error).

To find out more, please see our [Background Tasks](~/ios/watchos/platform/background-tasks.md) guide.

<a name="Complications-Enhancements"></a>

## Complications Enhancements

Complications are small visual elements that provide useful information at a glance. Depending on the watch face selected, the user has the ability to customize a watch face with one or more Complication.

watchOS 3 gives the app the ability to create one or more Complication for the watch app so that the user can access its information at-a-glance from a watch face.

Additionally, Complications provide the following benefits:

- The user can quickly launch the app by tapping on the Complication directly from a watch face.
- Having one of the app's Complications on the watch face causes the system to keep the app in a ready-to-launch state where it attempts to launch the app in the background, keep it in memory and gives it extra time to update.
- Complications are guaranteed at least 50 push updates per day.
- When the app includes Complications, it will be featured in the Apple Watch Face Gallery.

In watchOS 3, the ClockKit framework now includes several new templates for extra large complications such as [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) and [CLKComplicationTemplateExtraLargeRingImage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Additionally, to create localizable text, use new methods of the [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) class.

To find out more, please see our [Quick Interaction Techniques for watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Newly-Available-Frameworks"></a>

## Newly Available Frameworks

watchOS 3 includes several existing Apple frameworks that were previously unavailable such as:

- **SceneKit** - Use SceneKit to include 3D models into the watch app's UI including most of the features available on other platforms like lighting, shading, animation, physics and particle systems. 3D spatial audio, custom Metal or OpenGL shaders, Core Image Filters and physically-based materials are not supported.
- **SpriteKit** - Use SpriteKit to render and animate sprites in the app watch app's UI including most of the features available on other platforms like actions, physics, lighting and particle systems. 3D spatial audio, video playback and Core Image Filters are not supported.
- **AVFoundation** - To manage and play audio.
- **CloudKit** - To move data between the watch app and iCloud containers.
- **Core Audio** - To manage data types for representing audio streams, complex buffers and time values.
- **GameKit** - To create social games.

<a name="Proactive-Suggestions"></a>

## Proactive Suggestions

watchOS 3 allows the app to proactively present information to the user within given contexts. To support this feature, the [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) now includes the `MapItem` property that lets the app provide location information for later use by other apps.

To find out more, please see our [Introduction to Proactive Suggestions](~/ios/watchos/platform/proactive-suggestions.md) guide.

<a name="Security-and-Privacy-Enhancements"></a>

## Security and Privacy Enhancements

Apple has made several enhancements to both security and privacy in watchOS 3 that will help the developer improve the security of their apps and ensure the end user's privacy.

As a result, apps running on watchOS 3 (or later) must statically declare their intent to access specific features or user information by entering one or more Privacy Specific Keys in their `Info.plist` files that explain to the user why the app wishes to gain access.

Since watchOS 3 shares these changes with iOS 10, please see our iOS 10 [Security and Privacy Enhancements](~/ios/app-fundamentals/security-privacy.md) guide for more information.

<a name="Snapshots-and-Dock"></a>

## Snapshots and Dock

In watchOS 3, Apple has added the Dock where users can pin their favorite apps and quickly access them. When the user presses the Side Button on the Apple Watch, a gallery of pinned app snapshots will be displayed. The user can swipe left or right to find the desired app, then tap the app to launch it replacing the snapshot with the running app's interface.

The system periodically takes snapshots of the app's UI and uses those snapshots to populate the Docs. watchOS gives the app the opportunity to update its content and UI before this snapshot is taken.

For more information, please see our [Background Tasks](~/ios/watchos/platform/background-tasks.md) guide and Apple's [WKSnapshotRefreshBackgroundTask Reference](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications"></a>

## User Notifications

The User Notification framework introduced in watchOS 3 supports the delivery of both local and remote notifications to the Apple Watch. Use this framework to schedule notifications based on specific conditions such as time of day or location and to receive and handle notifications.

To find out more, please see our [Quick Interaction Techniques for watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Watch-Connectivity-Framework-Enhancements"></a>

## Watch Connectivity Framework Enhancements

The new `HasContentPending` property of the [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) class indicates that the session has received data in the background that needs to be processed. And the `RemainingComplicationUserInfoTransfers` property returns the remaining times that the iOS app can update its watchOS Complication.

To find out more, please see our [Background Tasks](~/ios/watchos/platform/background-tasks.md) guide.

<a name="WatchKit-Framework-Enhancements"></a>

## WatchKit Framework Enhancements

watchOS 3 includes several enhancements to the WatchKit framework including the following:

- The app can get the state of the Digital Crown using the new [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) class and receive updates when the user rotates the crown using the [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) class.
- The [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) class now includes the `ApplicationState` method and [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) constant that the app can use to track the runtime state of the app. `WKExtension` also provides two new methods that can be used to schedule background tasks.
- The [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) now includes the new `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` and `HandleBackgroundTasks` methods to monitor changes in the app's state and handle background task updates.
- A new [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) class has been added to provide the following types of gesture recognition to the watch apps: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) and [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- The new [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) class provides an interface for any HomeKit attached IP camera.
- The new [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) class allows the app to display a movie "poster" that is replaced by the running movie when the user taps it.
- The new [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) class allows the app to present an Apple Pay button in its UI that will initiate a payment request when tapped.
- The new [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) class presents an interface for displaying a SceneKit scene on the Apple Watch.
- The new [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) class presents an interface for displaying a SpriteKit scene on the Apple Watch.

To find out more, please see our [Quick Interaction Techniques for watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Workout-App-Enhancements"></a>

## Workout App Enhancements

New to watchOS 3, workout related apps have the ability to run in the background on the Apple Watch. To enable this feature (and gain access to HealthKit data), the app must include the `WKBackgroundModes` key in the `Info.plist` file with the value `workout-processing`.

Additionally, the developer now has the ability to launch the watchOS workout app from the iOS app version on the paired iPhone.

To find out more, please see our [Workout App Enhancements](~/ios/watchos/platform/workout-apps.md) guide.

<a name="Additional-Framework-Changes"></a>

## Additional Framework Changes

In addition to the major framework changes and additions listed above, Apple has made many additional minor framework changes in watchOS 3.

To find out more, please see our [Additional Framework Changes](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) guide.

<a name="Deprecated-APIs"></a>

## Deprecated APIs

The following APIs have been deprecated in watchOS 3:

- The `UILocalNotification` class of UIKit has been deprecated and should be replaced with the User Notification framework.

See Apple's [watchOS 2.2 to watchOS 3.0 API Differences](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) documentation for a complete list of deprecations and changes.

## Related Links

- [watchOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2bwatchOS)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)