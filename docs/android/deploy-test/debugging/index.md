---
title: "Debugging Xamarin.Android on Devices and Emulators"
description: "How to test and debug your Xamarin.Android app"
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
---

# Debugging

This section discusses how to debug a Xamarin.Android app on devices or emulators.

## Debugging Overview

Developing Android applications requires running the application,
either on physical hardware or using an emulator. Using
hardware is the best approach, but not always the most practical. In
many cases, it can be simpler and more cost effective to
simulate/emulate Android hardware using one of the emulators
described below.

### [Debugging on the Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)

This article explains how launch the Android emulator from Visual
Studio and run your app in a virtual device.

### [Debugging on a Device](~/android/deploy-test/debugging/debug-on-device.md)

This article shows how to configure a physical Android device so that
Xamarin.Android application can be deployed to it directly from either
Visual Studio or Visual Studio or Mac.

### [Android Debug Log](~/android/deploy-test/debugging/android-debug-log.md)

One very common trick developers use to debug their applications 
is using `Console.WriteLine`. However, on a mobile platform like Android
there is no console. Android devices provides a log that you will
likely need to utilize while writing apps. This is sometimes referred
to as **logcat** due to the command typed to retrieve it. This article
describes how to use **logcat**.

> [!WARNING]
> Note that the **Xamarin Android Player** has been deprecated. For more information, see the [announcement in this
 blog post](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/). In addition, the
**Visual Studio Android Emulator** has been deprecated as of Visual Studio 2017.
