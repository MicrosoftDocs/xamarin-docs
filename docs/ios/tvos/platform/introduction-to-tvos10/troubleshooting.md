---
title: "Troubleshooting tvOS 10 Apps Built With Xamarin"
description: "This article provides several troubleshooting tips for working with tvOS 10 in Xamarin apps. It describes issues related to the App Store, binary compatibility, the CFNetwork HttpProtocol, CloudKit, Core Image, NSUserActivity, and UIKit."
ms.service: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# Troubleshooting tvOS 10 Apps Built With Xamarin

The following sections list some known issues that can occur when using tvOS 10 with Xamarin and the solution to those issues:

- [App Store](#App-Store)
- [Binary Compatibility](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core Image](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store"></a>

## App Store

Known Issues:

- When testing In-App Purchases in the sandbox environment, the authentication dialog may appear twice.
- When testing In-App Purchases with hosted content in the sandbox environment, the password dialog will appear every time the app is brought to the foreground until the content download completes.

<a name="Binary-Compatibility"></a>

## Binary Compatibility

Known Issues:

- Calling `NSObject.ValueForKey` will a `null` key will result in an exception.
- Referencing a font by name when calling `UIFont.WithName` will cause a crash.
- Both `NSURLSession` and `NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://` URLs.
- Apps can hang if they modify a superview's geometry in either the `ViewWillLayoutSubviews` or `LayoutSubviews` methods.
- For all SSL/TLS connections, the RC4 symmetric cipher is now disabled by default. Additionally, the Secure Transport API no longer supports SSLv3 and it is recommended that the app stop using SHA-1 and 3DES cryptography as soon as possible.

<a name="CFNetwork-HTTP-Protocol"></a>

## CFNetwork HTTP Protocol

The `HTTPBodyStream` property of the `NSMutableURLRequest` class must be set to an unopened stream since `NSURLConnection` and `NSURLSession` now strictly enforce this requirement.

<a name="CloudKit"></a>

## CloudKit

Long running operations will return a _"You don't have permission to save the file."_ error.

<a name="CoreImage"></a>

## Core Image

The `CIImageProcessor` API now supports an arbitrary input image count. `CIImageProcessor` API that was included in tvOS 10 beta 1 will be removed.

<a name="NSUserActivity"></a>

## NSUserActivity

After a Handoff operation, the `UserInfo` property of a `NSUserActivity` object might be empty. Explicitly call `BecomeCurrent` NSUserActivity` object as a current workaround.

<a name="UIKit"></a>

## UIKit

Known Issues:

- Changes to the background appearance of `UINavigationBar`, `UITabBar` or `UIToolBar` may result in a layout pass to resolve the new appearance. Attempting to modify these appearances inside of a `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` or `DidUpdateSubviews` event can result in an infinite layout loop.
- In tvOS 10, calling the `RemoveGestureRecognizer` method of a `UIView` object explicitly cancels any in-progress Gesture Recognizer.
- Presented View Controllers can now affect the appearance of the status bar.
- tvOS 10 requires the developer to call `base.AwakeFromNib` when subclassing `UIViewController` and overriding the `AwakeFromNib` method.
- Apps with custom `UIView` subclasses that override `LayoutSubviews` and dirty the layout before calling `base.LayoutSubviews` may trigger an infinite layout loop in tvOS 10.
- Direction-specific or flippable images assets are no flipping when assigned to `UIButton` objects.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [What's new in tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)