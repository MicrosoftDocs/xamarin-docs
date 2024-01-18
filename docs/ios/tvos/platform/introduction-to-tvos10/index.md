---
title: "Introduction to tvOS 10"
description: "This article introduces all of the new and modified APIs and features available in tvOS 10 for Xamarin.tvOS developers."
ms.service: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# Introduction to tvOS 10

_This article introduces all of the new and modified APIs and features available in tvOS 10 for Xamarin.tvOS developers._

With the new tvOS 10 SDK Apple has included new APIs and services that enable the developer to create new categories of apps and features. 

For more information on tvOS 10, please see Apple's [tvOS + Apps](https://developer.apple.com/tvos/) documentation.

## What's New in tvOS 10

Apple has added several new APIs and services in tvOS 10 along with many enhancements to existing features, including:

## New User Interface Styles

tvOS 10 now supports both a Dark and Light User Interface theme that all of the build-in UIKit controls will automatically adapt to, based on the user's preferences.

When creating and implementing new custom UI controls, the developer should use the [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) class to adapt to the user's selected theme.

For more information, please see our [New User Interface Styles](~/ios/tvos/platform/user-interface-styles.md) documentation.

## Security and Privacy Enhancements

Apple has made several enhancements to both security and privacy in tvOS 10 that will help the developer improve the security of their apps and ensure the end user's privacy.

As a result, apps running on watchOS 3 (or later) must statically declare their intent to access specific features or user information by entering one or more Privacy Specific Keys in their `Info.plist` files that explain to the user why the app wishes to gain access.

Since tvOS 10 shares these changes with iOS 10, please see our iOS 10 [Security and Privacy Enhancements](~/ios/app-fundamentals/security-privacy.md) guide for more information.

## Video Subscriber Account

New for tvOS 10, the Video Subscriber Account framework allows apps that support authenticated streaming or video-on-demand to authenticate with their cable or satellite TV provider using a Single-Sign-in experience for the end user.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## Wide Color

tvOS 10 extends the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

Additionally, `UIKit` has been modified to work in the new extended **sRGB** colorspace, making it easier to mix colors in wide color gamuts without significant performance loss.

Apple offers the following best practices when working with wide colors:

- `UIColor` now uses the sRGB color space and will no longer clamp values to the `0.0` to `1.0` range. If the app relies on the previous clamp behavior, it will need to be modified for tvOS 10.
- If the app performs custom rendering of `UIImages`, use the new [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) class to specify the use of the extended-range or standard-range formats.
- When using a low-level API such as Core Graphics or Metal to provide image processing, the app should use an extended range color space and pixel format that supports 16-bit floating point values. Where necessary, the app will have to manually clamp color component values.
- Core Graphics, Core Image and Metal Performance Shaders all provide new methods for converting between the two color spaces.

To find out more, please see our [Introduction to Wide Color](~/ios/platform/wide-color.md) guide.

## Newly Available Existing Frameworks

Several frameworks that were available on iOS (and not tvOS), have been made available for tvOS 10 such as:

- ExternalAccessory
- HomeKit
- MultipeerConnectivity
- Photos
- ReplayKit
- UserNotification

## Additional Framework Changes

In addition to the major framework changes and additions listed above, Apple has made many additional minor framework changes in tvOS 10.

To find out more, please see our [Additional Framework Changes](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) guide.

## Deprecated APIs

No APIs or frameworks were deprecated by tvOS 10. See Apple's [tvOS 10 API Differences](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) documentation for a complete list of API modifications.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [What's new in tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)