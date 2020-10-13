---
title: "What USB drivers do I need to debug Android on Windows?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
---

# What USB drivers do I need to debug Android on Windows?

## Finding USB Drivers

To debug on an Android device when developing in Windows; you need to
install a compatible USB driver. The Android SDK Manager includes the
"Google USB Driver" by default, which adds support for Nexus devices as
described here:
[https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

Other devices require USB drivers specifically published by the device
manufacturer. Some links for the most common manufacturers are included
in this guide:
[https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## Alternatives

Depending on the manfacturer, it can be difficult to track down the
exact USB driver needed. Some alternatives for testing Android apps
developed in Windows including using an Android emulator or using
external testing services. Some of these include:

- [App Center Test](/appcenter/test-cloud/) - Cloud Testing services run on hundreds of real Android devices.

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debugging on the Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)