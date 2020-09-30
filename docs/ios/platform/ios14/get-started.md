---
title: "Get started with iOS 14"
description: "This document describes how to get set up to build iOS 14 apps with Xamarin. It discusses how to download Xcode 12 and update Visual Studio for Mac."
ms.prod: xamarin
ms.assetid: 0d721b4b-86bd-495a-8c0f-1f2f9fd6855e
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2020
---

# Get started with iOS 14

This document describes how to start building Xamarin apps that call APIs released with Xcode 12 for iOS 14. Using the preview requires macOS Catalina 10.15.4 or later.

## Download and install

1. **Install Xcode 12** –
   Registered Apple developers can download and install the latest version
   of the Xcode 12 from the
   [Apple Developer Portal](https://developer.apple.com/download/) or the **App Store**.

2. **Run Xcode 12** – Run Xcode 12 before updating and running Visual
   Studio for Mac or Visual Studio 2019, as it installs some tools that Xamarin requires.

3. **Update Visual Studio for Mac and Visual Studio 2019** – Ensure you have the latest stable version of Xamarin.

4. (Optional) **Install iOS 14 on your iOS devices** – For device testing of apps 
   that use APIs introduced with the Xcode 12,
   registered Apple developers can [download](https://developer.apple.com/download)
   and install the operating system on their devices. 

   > [!TIP]
   > Even if your app does not use any new APIs, be sure to build it with
   > the newest Xcode 12 SDKs and test it to make sure that everything works
   > as expected. If an app doesn't call any new APIs, you can recompile it
   > with these new SDKs and test it on devices that have not yet been
   > upgraded to the new operating system.
   >
   > Before upgrading your devices to the latest operating system releases
   > from Apple to test your Xamarin apps, be sure to:
   >
   > - Read [Apple's release notes](https://developer.apple.com/download/)
   >   for the operating system updates.

## Related links

- [Download Xcode](https://developer.apple.com/download/)
- [Xamarin.iOS release notes](/xamarin/ios/release-notes/14/14.0)
- [Xcode 12 release notes](https://developer.apple.com/documentation/xcode-release-notes/xcode-12-release-notes)
