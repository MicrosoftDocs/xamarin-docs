---
title: "Get started with iOS 13, iPadOS 13, tvOS 13, and watchOS 6"
description: "This document describes how to get set up to build iOS 13, iPadOS 13, tvOS 13, and watchOS 6 apps with Xamarin. It discusses how to download Xcode 11 and update Visual Studio for Mac and Visual Studio 2019."
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
---
# Get started with iOS 13

![Preview feature](~/media/shared/preview.png)

This document describes how to start building Xamarin apps that call
APIs released with Xcode 11, for iOS 13. Using the preview requires macOS 10.14.4 (Mojave) or newer.

## Download and install

1. **Install Xcode 11 beta** –
   Registered Apple developers can download and install the latest version
   of the Xcode 11 beta from the
   [Apple Developer Portal](https://developer.apple.com/download/) or the **App Store**.

2. **Run Xcode 11 beta** – Run Xcode 11 before updating and running Visual
   Studio for Mac, as it installs some tools that Xamarin requires.

3. In Visual Studio for Mac, select **Visual Studio > Check for Updates...**, 
   select the **Xcode 11 Previews** channel, and install the available updates.

4. In Visual Studio for Mac, select **Visual Studio > Preferences > Projects > SDK Locations > Apple** and select **Xcode-beta.app**.

5. (Optional) If evaluating this preview using _Xcode 11 beta 3_, you must enable linking. Right-click your project, navigate to **Options > iOS Build > Linker behavior** and set the linker behavior value to **Link Framework SDKs Only**. This workaround will not be necessary in a future preview.

6. (Optional) **Install iOS 13 on your iOS devices** – For device testing of apps that use APIs introduced with the Xcode 11,
   registered Apple developers can [download](https://developer.apple.com/download)
   and install the operating system on their devices. Because iOS is in beta, be careful installing it on your primary device.

   > [!TIP]
   > Even if your app does not use any new APIs, be sure to build it with
   > the newest Xcode 11 SDKs and test it to make sure that everything works
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
