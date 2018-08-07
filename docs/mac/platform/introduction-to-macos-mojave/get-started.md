---
title: "Getting started with macOS Mojave"
description: "This document describes how to get set up to build macOS Mojave apps with Xamarin.Mac. It discusses how to download Xcode 10 and update Visual Studio for Mac."
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
---
# Getting started with macOS Mojave

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Xamarin's macOS Mojave support is currently in preview, which means
> that it may contain bugs, is not feature-complete, and may change.
> Use it for experimentation only. 

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

3. **Update Visual Studio for Mac** – Follow the
   instructions on the [release blog](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
   to install the Xamarin preview.

4. _(optional)_ **Install the latest macOS Mojave beta on your Mac** –
   To test Xamarin.Mac apps that use newly-introduced macOS Mojave APIs,
   registered Apple developers can [download](https://developer.apple.com/download/)
   and install the latest macOS Mojave developer beta.

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
   > - Read the Xamarin preview
   >   [release blog post](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## Related links

- [Download Xcode 10](https://developer.apple.com/download/)
- Xamarin preview [release blog post](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
