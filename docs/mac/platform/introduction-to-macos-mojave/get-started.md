---
title: "Get started with macOS Mojave"
description: "This document describes how to get set up to build macOS Mojave apps with Xamarin.Mac. It discusses how to download Xcode 10 and update Visual Studio for Mac."
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
---
# Get started with macOS Mojave

This document describes how to get set up to build macOS Mojave apps with
Xamarin.Mac. It discusses how to download Xcode 10 and update Visual Studio
for Mac.

## Download and install

1. **Install the latest Xcode 10 beta** –
   Registered Apple developers can download and install the latest version
   of Xcode 10 from the
   [Apple Developer Portal](https://developer.apple.com/download/).

2. **Run Xcode 10** – Run Xcode 10 before updating and running Visual
   Studio for Mac; it installs some tools that Xamarin requires.

3. **Update Visual Studio for Mac** – Use the latest stable version of Visual Studio for Mac, with [Xamarin.Mac 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/mac/xamarin.mac_5/xamarin.mac_5.0.md) or newer.

4. _(optional)_ **Install the macOS Mojave on your Mac** –

   > [!TIP]
   > Even if your app does not use any new macOS Mojave APIs, be sure to
   > build it with the macOS Mojave SDK and test it to make sure that
   > everything works as expected. If an app doesn't call any new APIs,
   > you can recompile it with the macOS Mojave SDK and test it without
   > upgrading your Mac's operating system.
   >
   > Before upgrading your Mac to macOS Mojave to build and test Xamarin.Mac
   > applications that call new macOS Mojave APIs:
   >
   > - Read [Apple's release notes](https://developer.apple.com/download/)
   >   for the operating system update.

## Related links

- [Download Xcode 10](https://developer.apple.com/download/)
- [Xamarin.Mac 5.0 release notes](/xamarin/mac/release-notes/5/5.0/)