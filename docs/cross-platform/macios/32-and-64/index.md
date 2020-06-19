---
title: "32/64-bit platform considerations"
description: "This document describes various considerations to keep in mind when targeting 32-bit and 64-bit architectures for a Xamarin.iOS or Xamarin.Mac application."
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
---

# 32/64-bit platform considerations

While iOS and macOS have historically supported both 32 and 64-bit apps,
Apple has gradually deprecated 32-bit support.

As of iOS 11, 32-bit apps will no longer launch, and 
[all submissions to the App Store must support 64-bit](https://developer.apple.com/news/?id=06282017b).

Starting in January 2018, [new apps submitted to the Mac App Store 
must support 64-bit](https://developer.apple.com/news/?id=06282017a), and
existing apps must be updated by June 2018.

Xamarin's Classic API (`XamMac.dll` and `monotouch.dll`) supported only
32-bit applications. However, new Xamarin.iOS and Xamarin.Mac applications 
use the [Unified API](~/cross-platform/macios/unified/index.md) 
(`Xamarin.iOS` and `Xamarin.Mac`) by default, and can therefore target both 
32 and 64-bit, as necessary.

## iOS

<a name="enable-64"></a>

### Enabling 64-bit builds of Xamarin.iOS apps

> [!WARNING]
> This section is included for historic reasons, and to help move older Xamarin.iOS projects to the Unified API and support 64-bit. All new Xamarin.iOS projects will use the Unified API and target 64-bit by default.

For Xamarin.iOS mobile applications that have been converted to the Unified API, developers must manually update the build settings to target 64-bit:

<!-- markdownlint-disable MD001 -->

# [Visual Studio for Mac](#tab/macos)

1. In the **Solution Pad**, double-click the app's project to open the **Project Options** window.
2. Select **iOS Build**.
3. For the iPhone Simulator, in the **Supported architectures** dropdown, select either **x86\_64** or **i386 + x86\_64**:

   [![Setting Supported architectures to x86\_64 or i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. For physical devices, select one of the available **ARM64** combinations:

   [![Setting Supported architectures to one of the ARM64 combinations](Images/Image02.png "Setting Supported architectures to one of the ARM64 combinations")](Images/Image02-large.png#lightbox)

5. Click **OK**.
6. Perform a clean build.

# [Visual Studio](#tab/windows)

1. In the **Solution Explorer**, right-click the app's project and select **Properties**.
2. Select **iOS Build**.
3. For the iPhone Simulator, set **Supported architectures** to either **x86\_64** or **i386 + x86\_64**: 

   [![Setting Supported architectures to x86_64 or i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. For physical devices, select one of the available **ARM64** combinations:
    
   [![Setting Supported architectures to one of the ARM64 combinations](Images/VS01.png "Setting Supported architectures to one of the ARM64 combinations")](Images/VS01-large.png#lightbox)

5. Save your changes.
6. Perform a clean build.

-----

ARMv7s is supported only by the A6 processor included in the iPhone 5 (or greater). ARMv7 code is faster and smaller than the ARMv6, only works with the iPhone 3GS and later, and is required by Apple when targeting the iPad or a minimum iOS version of 5.0. ARMv6 works on all devices but is no longer supported by the compiler shipped with Xcode 4.5 and later. 

ARM64 is required to support iOS 8 on iPhone 6 or other 64-bit devices and will be required by Apple when submitting new or updating exiting applications in the iTunes App Store.

For a comprehensive look at the capabilities of various iOS devices, check out Apple's
[Device Compatibility](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) document.

### 64-bit and binary size increases

During Apple's transition from 32-bit to 64-bit, iOS apps will need to run on both 32-bit and 64-bit hardware. Because of this, Xamarin's Unified API allows developers to target both.

Targeting both 32-bit and 64-bit architectures will significantly increase the size of an application. However, doing so will allow newer devices to run optimized code while still supporting older devices.

> [!IMPORTANT]
> If you receive the following message when submitting an iOS application to the iTunes App Store, _"WARNING ITMS-9000: Missing 64-bit support. Starting February 1, 2015, new iOS apps uploaded to the App Store must include 64-bit support and be built with the iOS 8 SDK, included in Xcode 6 or later. To enable 64-bit in your project, we recommend using the default Xcode build setting of “Standard architectures” to build a single binary with both 32-bit and 64-bit code."_ You need to switch the supported architectures to one of the available **ARM64** combination (as shown above), recompile and resubmit.

## Mac

> [!IMPORTANT]
> Starting in January 2018, all new Mac apps submitted to the 
> Mac App Store must support 64-bit. Existing Mac App Store apps and their 
> updates must support 64-bit starting in June 2018. See 
> [Apple's announcment](https://developer.apple.com/news/?id=06282017a) 
> and [a guide that describes how to update your Xamarin.Mac apps to 64-bit](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Most modern Mac computers support both 32-bit and 64-bit
    applications.   MacOS 10.6 (Snow Leopard) was the last
    operating system to run on 32-bit systems.   Most Macs
    released since 2010 support both systems.

Unlike iOS, many of the new frameworks introduced in recent
    versions of macOS are only supported in 64-bit mode (CloudKit,
    EventKit, GameController, LocalAuthentication, MediaLibrary,
    MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit,
    Social, and MapKit, among others).

The Unified API allow developers to choose what kind of
    applications they want to produce: 32-bit or 64-bit.

**32-bit applications** will run on both 32-bit and
    64-bit Mac computers, have an address space limited to 32
    bits, and require that all libraries are 32 bits.

You will typically use this mode if you have 32-bit
    dependencies that do not run in 64-bit mode, if you want to
    have a smaller download, or if there are no performance
    benefits in moving to 64-bit.

This mode is limiting as you wont be able to use many
    frameworks available in macOS Mavericks and macOS Yosemite.

**64-bit applications** will only run on 64-bit Mac
    devices.

For Mac, this is the preferred mode of operation as most
    Macs in use today support 64-bit mode, and you have access to
    the complete set of frameworks provided by Apple.

### Enabling 64-bit builds of Xamarin.Mac apps

For information about building a 64-bit app using Xamarin.Mac,
please see the [Updating Xamarin.Mac Unified applications to 64-bit](~/cross-platform/macios/32-and-64/mac-64-bit.md) 
guide.

## Related links

- [Classic vs Unified API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
