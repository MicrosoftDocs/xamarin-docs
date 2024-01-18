---
title: "Introduction to macOS Sierra"
description: "This article introduces all of the new and modified APIs and features available in macOS Sierra for Xamarin.Mac developers."
ms.service: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
no-loc: [Objective-C]
---

# Introduction to macOS Sierra

With the new macOS Sierra, the developer can take advantage of new APIs that allow the end user to interact with their apps and websites in previously unavailable ways. For example, Apple now allows websites to give customers the option of paying securely via Apple Pay and enhancements to the Metal framework boost an app's graphics and computing potential. 

For more information on macOS Sierra, please see Apple's [macOS + Apps](https://developer.apple.com/macos/) documentation.

<a name="Whats-New-in-macOS-Sierra"></a>

## What's New in macOS Sierra

Apple has added several new APIs and services in macOS Sierra along with many enhancements to existing features, including:

<a name="Apple-File-System"></a>

### Apple File System

With macOS Sierra, Apple has released the new Apple File System as a modern file system for iOS, macOS, tvOS and watchOS. The Apple File System was optimized for Flash and SSD storage and provides the following features: strong encryption, copy-on-write metadata, space sharing, cloning for files and directories, snapshots, fast directory sizing and atomic safe-save primitives.

For more information, please see Apple's [Apple File System Guide](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements"></a>

### Apple Pay Enhancements

Apple has made several enhancements to Apple Pay in macOS Sierra that allow the user to make secure payments from websites.

With macOS Sierra, several new APIs have been added that work with macOS Sierra, iOS and watchOS to support dynamic payment networks and a new sandbox test environment.

macOS Sierra includes the new ApplePay Javascript framework that allows the developer to incorporate Apple Pay directly into iOS and macOS Safari-based websites. For websites that support Apple Pay, the user can authorize payment using either their iPhone or Apple Watch.

For more information, please see Apple's [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) reference.

<a name="Building-Modern-macOS-Apps"></a>

### Building Modern macOS Apps

Modern macOS apps such as Apple's Safari web browser, Pages word processor and Numbers spread sheet use many new technologies to present a unified, context sensitive User Interface that does away with traditional UI elements such as floating panels and multiple open windows.

[![An example of a tabbed Mac window](images/content08.png)](images/content08.png#lightbox)

Our [Building Modern macOS Apps](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) guide covers several tips, features and techniques a developer can use to build a modern macOS app in Xamarin.Mac.

<a name="CloudKit-Data-Sharing"></a>

### CloudKit Data Sharing

The CloudKit framework has been expanded in macOS Sierra to allow user to quickly and easily share records or record sets from their private iCloud databases.

CloudKit provides a complete UI for sending and accepting shared record invitations and the user has complete read/write control over the people that have access to the records.

For more information, please see Apple's [CloudKit Framework Reference](https://developer.apple.com/reference/clockkit) and [CloudKit JS Framework Reference](https://developer.apple.com/reference/cloudkitjs).

> [!IMPORTANT]
> Apple [provides tools](https://developer.apple.com/support/allowing-users-to-manage-data/) 
> to help developers properly handle the European Union's General Data 
> Protection Regulation (GDPR).

<a name="Safari-App-Extensions-Support"></a>

### Safari App Extensions Support

Safari App Extensions allow the app to extend the behavior of the Safari web browser while being tightly integrated with macOS Sierra. Since macOS Safari App Extensions work similar to iOS Safari App Extensions, they are easy to port from one system to another.

For more information, please see Apple's [Safari App Extension Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements"></a>

### Security and Privacy Enhancements

Apple has made several enhancements to both security and privacy in macOS Sierra that will help the app improve the security of the app and ensure the end user's privacy including the following:

- The new `NSAllowsArbitraryLoadsInWebContent` key can be add to the app's `Info.plist` file and will allow web pages to load correctly while Apple Transport Security (ATS) protection is still enabled for the rest of the app.
- The Common Data Security Architecture (CDSA) API has been deprecated and should be replaced with the SecKey API to generate asymmetric keys.
- For all SSL/TLS connections, the RC4 symmetric cipher is now disabled by default. Additionally, the Secure Transport API no longer supports SSLv3 and it is recommended that the app stop using SHA-1 and 3DES cryptography as soon as possible.
- Because the new Clipboard in iOS 10 and macOS Sierra allows the user to copy and paste between devices, the API has been expanded to allow a clipboard to be limited to a specific device and be timestamped to be cleared automatically at a given point. Additionally, named pasteboards are no longer persisted and should be replaced with the shared pasteboard containers.
- If the app accesses protected data (such as the user's Calendar), it _must_ declare that intent with the correct purpose string value key in its `Info.plist` file (`NSCalendarUsageDescription` in the case of the Calendar).
- Developer Signed apps that are not delivered via the Mac App Store can now take advantage of CloudKit, iCloud Keychain, iCloud Drive, remote push notifications, MapKit and VPN entitlements.
- macOS Sierra no longer supports delivering external code or data along with the code-signer app in its zip archive or unsigned disk image as the runtime path is not known before runtime.

Additionally, apps running on macOS Sierra (or later) must statically declare their intent to access specific features or user information by entering one or more Privacy Specific Keys in their `Info.plist` files that explain to the user why the app wishes to gain access.

Since macOS Sierra shares these changes with iOS 10, please see our iOS 10 [Security and Privacy Enhancements](~/ios/app-fundamentals/security-privacy.md) guide for more information.

<a name="Smart-Card-Driver-Extension-Support"></a>

### Smart Card Driver Extension Support

With macOS Sierra, the app can create `NSExtension` based smart card drivers that allows read-only access to the content from certain types of smart cards. This information is then presented inside the system keychain (replacing the deprecated Common Data Security Architecture method).

for more information, Pleas see Apple's [CryptoTokenKit Framework Reference](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging"></a>

### Unified Logging

Unified Logging provides the app with a single API for efficient messaging across all levels of the system. With Unified Logging the app has fine-grained control over multiple levels of logging that include privacy controls and activity tracking for easier debugging. 

Logging provides automatic message correlation when activity tracking and logging are used together.

macOS Sierra includes a new Console App (in Applications/Utilities) that is able to display log data from multiple sources including connected devices. It also supports tokenized and saved searches and displays connections between related messages across multiple processes.

Additionally, log messages can be viewed and maintained using command line tools.

For more information, please see Apple's [Logging Reference](https://developer.apple.com/documentation/os/logging).

<a name="Wide-Color"></a>

### Wide Color

macOS Sierra extends the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

Additionally, `AppKit` has been modified to work in the new extended **sRGB** colorspace, making it easier to mix colors in wide color gamuts without significant performance loss.

Apple offers the following best practices when working with wide colors:

- `NSColor` now uses the sRGB color space and will no longer clamp values to the `0.0` to `1.0` range. If the app relies on the previous clamp behavior, it will need to be modified for macOS Sierra.
- When using a low-level API such as Core Graphics or Metal to provide image processing, the app should use an extended range color space and pixel format that supports 16-bit floating point values. Where necessary, the app will have to manually clamp color component values.
- Core Graphics, Core Image and Metal Performance Shaders all provide new methods for converting between the two color spaces.

To find out more, please see our [Introduction to Wide Color](~/ios/platform/wide-color.md) guide.

<a name="Additional-Framework-Changes"></a>

## Additional Framework Changes

In addition to the major framework changes and additions listed above, Apple has made many additional minor framework changes in macOS Sierra.

To find out more, please see our [Additional Framework Changes](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) guide.

<a name="Deprecated-APIs"></a>

## Deprecated APIs

The following APIs have been deprecated in macOS Sierra:

- The HFS Standard File System is no longer supported.

See Apple's [macOS v10.12 API Diffs](https://developer.apple.com/library/archive/releasenotes/General/APIDiffsMacOS10_12/index.html) documentation for a complete list of deprecations and changes.

## Related Links

- [Mac Samples](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [What's new in macOS 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)