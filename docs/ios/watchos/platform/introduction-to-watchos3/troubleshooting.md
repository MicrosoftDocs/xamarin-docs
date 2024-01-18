---
title: "watchOS 3 Troubleshooting"
description: "This document provides several troubleshooting tips useful when working with watchOS 3 in Xamarin. Tips relate to activities, Apple Pay, background refresh, NSURLConnection, privacy, and more."
ms.service: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
no-loc: [Objective-C]
---

# watchOS 3 Troubleshooting

_This article provides several troubleshooting tips for working with watchOS 3 in Xamarin Apple Watch apps._

This page lists some known issues that can occur when using watchOS 3 with Xamarin and the solution to those issues.

## Activities

For Activity sharing to work correctly, all paired Apple Watches must be running watchOS 3.

Known Issues:

- Replying to an Activity Sharing Notification sometimes fails.
- Replying to an Activity Sharing Notification with a message may fail.
- Contextual text above an Activity Sharing Notification message will be incorrect.

## Apple Pay

Known Issues:

- If an incorrect expiration date or CW code is entered for a new payment care in Apple Pay, when hitting **Next** the running process will crash.
- In-App Apple Pay purchases requiring a PIN number may crash.

## Auto Mac Unlock

By using watchOS 3 beta 2 (or greater) and macOS Sierra beta 2 (or greater), if two-factor authentication is enabled on the user's iCloud account, they can use their Apple Watch to auto unlock their Mac.

## Background Refresh

Violating system resources will result in a watchOS 3 app crash with the following exception codes:

- **0xc51bad01** - The app consumed too much CPU time.
- **0xc51bad02** - The app consumed too much wall time.
- **0xc51bad03** - The app did not have enough runtime to complete the current task.

## Clock

Complications from newly installed Apple Watch apps may show up as blank. Reboot Apple Watch to fix this issue.

## Connectivity

Known Issues:

- watchOS will not prompt the user for access permission for protected user data on the Apple Watch. Grant access on the iPhone app before using data in the watch app.
- The Apple Watch can enter a state where all WatchConnectivity transmissions fail, reboot the Apple Watch to fix.

## Notifications

If a media attachment is too large, it will be presented on the user's iPhone but not their Apple Watch.

## NSURLConnection

Any `NSURLConnection` connections using older TLS protocols will fail. For all SSL/TLS connections, the RC4 symmetric cipher is now disabled by default. Additionally, the Secure Transport API no longer supports SSLv3 and it is recommended that the app stop using SHA-1 and 3DES cryptography as soon as possible.

As of watchOS 3, SSL/TLS connections security is being strictly enforced by Apple. Affected services and apps should updated web servers to use the latest TLS protocol versions.

## NSURLSession

As of watchOS 3, the `HTTPBodyStream` property of the `NSMutableURLRequest` class must be set to an unopened stream since `NSURLConnection` and `NSURLSession` now strictly enforce this requirement.

## Privacy

Known Issues:

When working with `https://` URLs both `NSURLSession` and `NSURLConnection` no longer support RC4 cipher suites during the TLS handshake. One of the following error codes may be generated:

- **-1200 or -98** - For `NSURLErrorSecurityConnectionFailed` and SecureTransport errors.
- **-1200 [3:-9824]** - Http load failed.
- **-1200** - `NSURLConnection` finished with error.

As of watchOS 3, SSL/TLS connections security is being strictly enforced by Apple. Affected services and apps should updated web servers to use the latest TLS protocol versions. See [NSURLConnection](#nsurlconnection) above for more information.

## Snapshots

WatchKit apps that have not adopted the new `HandelBackgroundTask` API will no longer receive periodic updates in watchOS 3. 

## WatchKit

SpriteKit and SceneKit scenes will be paused when an app enters the background in the watchOS Dock.

## Related Links

- [watchOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2bwatchOS)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)