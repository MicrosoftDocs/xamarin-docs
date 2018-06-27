---
title: "Getting Started with iOS 12 and tvOS 12"
description: "This document describes how to get set up to build iOS 12 and tvOS 12 apps with Xamarin. It discusses how to download Xcode 10 and update Visual Studio for Mac and Visual Studio 2017."
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
---
# Getting started with iOS 12 and tvOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Xamarin's iOS 12 and tvOS 12 support is currently in preview,
> which means that it may contain bugs, is not feature-complete, and may
> change. Use it for experimentation only.

> [!NOTE]
> For more information, read the
> [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
> for the Xamarin preview release.

This document describes how to get set up to build iOS 12 and
tvOS 12 apps with Xamarin. It discusses how to download Xcode 10 and update
Visual Studio for Mac and Visual Studio 2017.

## Download and install

1. **Install the latest Xcode 10 beta** –
   Registered Apple developers can download and install the latest version
   of Xcode 10 from the
   [Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > The iOS 12 and tvOS 12 SDKs are distributed with Xcode 10.

2. **Run Xcode 10** – Run Xcode 10 before updating and running Visual
   Studio for Mac or Visual Studio 2017; it installs some tools that Xamarin
   requires.

3. **Update Visual Studio for Mac and Visual Studio 2017** – Follow the
   instructions in the [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
   to install the Xamarin preview.

4. _(optional)_ **Install the latest iOS beta on your iOS devices** –
   For device testing of apps that use newly-introduced iOS 12 or tvOS
   12 APIs, registered Apple developers can [download](https://developer.apple.com/download)
   and install the latest iOS 12 or tvOS 12 developer betas on their
   iOS and tvOS devices.

   > [!TIP]
   > Even if your app does not use any new iOS 12 or tvOS 12 APIs,
   > be sure to build it with the iOS 12 or tvOS 12 SDK (installed
   > with Xcode 10) and test it to make sure that everything works as
   > expected. If an app doesn't call any new APIs, you can recompile
   > it with the iOS 12 or tvOS 12 SDK and test it on devices that have
   > not yet been upgraded to the new operating systems.

   > [!IMPORTANT]
   > Before upgrading your devices to iOS 12 or tvOS 12 to test Xamarin
   > applications that call new iOS 12 or tvOS 12 APIs:
   > - Read [Apple's release notes](https://developer.apple.com/download/)
   >   for the operating system update.
   > - Read the [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
   >   for the Xamarin preview release.

## Related links

- [Download Xcode 10](https://developer.apple.com/download/)
- Xamarin preview [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
