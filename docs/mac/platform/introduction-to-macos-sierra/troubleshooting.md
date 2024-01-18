---
title: "Xamarin.Mac - macOS Sierra Troubleshooting"
description: "This document provides several troubleshooting tips for working with macOS Sierra in Xamarin.Mac apps. Tips relate to the Mac App Store, Apple Pay, binary compatibility, CFNetwork, CloudKit, and more."
ms.service: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 09/22/2016
no-loc: [Objective-C]
---

# Xamarin.Mac - macOS Sierra Troubleshooting

_This article provides several troubleshooting tips for working with macOS Sierra in Xamarin.Mac apps._

The following sections list some known issues that can occur when using macOS Sierra with Xamarin.Mac and the solution to those issues:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Binary Compatibility](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core Image](#CoreImage)
- [Notifications](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store"></a>

## App Store

Known Issues:

- When testing In-App Purchases in the sandbox environment, the authentication dialog may appear twice.
- When testing In-App Purchases with hosted content in the sandbox environment, the password dialog will appear every time the app is brought to the foreground until the content download completes.

<a name="Apple-Pay"></a>

## Apple Pay

If an incorrect expiration date or security code (CW) is entered when adding a new payment card to Apple Pay, the card provisioning process will be terminated.

<a name="Binary-Compatibility"></a>

## Binary Compatibility

Known Issues:

- Calling `NSObject.ValueForKey` will a `null` key will result in an exception.
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

The `CIImageProcessor` API now supports an arbitrary input image count. `CIImageProcessor` API that was included in macOS Sierra beta 1 will be removed.

<a name="Notifications"></a>

## Notifications

When working with Notification Content Extensions, View Controllers are not being correctly released and might result in a crash when Extension memory limits are reached.

<a name="NSUserActivity"></a>

## NSUserActivity

After a Handoff operation, the `UserInfo` property of a `NSUserActivity` object might be empty. Explicitly call `BecomeCurrent` `NSUserActivity` object as a current workaround.

<a name="Safari"></a>

## Safari

WebGeolocation requires a secure (`https://`) URL to work on both iOS 10 and macOS Sierra to prevent malicious use of location data.

## Related Links

- [Mac Samples](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [What's new in macOS 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)