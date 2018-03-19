---
title: "Debugging"
description: "How to test and debug your Xamarin.Android app"
ms.topic: article
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
---

# Debugging

## Debugging Overview

Developing Android applications requires running the application,
either on physical hardware or using an emulator or simulator. Using
hardware is the best approach, but not always the most practical. In
many cases, it can be simpler and more cost effective to
simulate/emulate Android hardware using one of the emulators
described below.


### [Android SDK Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

These articles explain how to use the default emulator that is provided
with the Android SDK. This emulator is available for Visual Studio for
Windows and Visual Studio for Mac.

### [Visual Studio Android Emulator](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

This article explains how to debug and test your Xamarin.Android app
using the Android emulator that is built into Visual Studio 2015. This
emulator is a good choice if you are using Visual Studio 2015 and do
not need custom device profiles.

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
 blog post](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
