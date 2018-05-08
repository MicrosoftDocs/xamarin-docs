---
title: "Android Emulator Setup"
description: "This section describes how to prepare the Google Android Emulator for testing your app. It explains how to accelerate the emulator for maximum performance, and it shows you how to use an emulator manager to create and customize virtual devices."
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
---

# Android Emulator Setup

_This section describes how to prepare the Google Android Emulator for testing your app. It explains how to accelerate the emulator for maximum performance, and it shows you how to use an emulator manager to create and customize virtual devices._


## Overview

The Google Android SDK emulator can be run in a variety of
configurations to simulate different devices. Each one of these
configurations is created as a _virtual device_. In this guide, you
will learn how to accelerate the Android emulator for better
performance and use either the Xamarin Android Emulator Manager or the
legacy Google Emulator Manager to create virtual devices.


> [!NOTE]
> As of Android SDK Tools version **26.0.1** and later, Google has removed support for existing AVD/SDK managers in favor of their new CLI (Command Line Interface) tools. Because of this deprecation change, Xamarin SDK/Device Managers are now used instead of Google SDK/Emulator Managers for Android Tools 26.0.1 and later. (For more information about the Xamarin SDK Manager, see [Android SDK Setup](~/android/get-started/installation/android-sdk.md)).


## Sections

### [Hardware Acceleration](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

How to prepare your computer for maximum Google Android Emulator
performance. Because the Google Android Emulator can be prohibitively slow
without hardware acceleration, we recommend that you enable hardware
acceleration on your computer before you use the Google Android Emulator.

### [Xamarin Android Device Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

How to use the Xamarin Android Device Manager to create and customize
Google Android Emulator virtual devices. The **Xamarin Android Device
Manager**, currently in preview, is intended to replace the legacy Google
Emulator Manager. If you are targeting Android Oreo 8.0 or later, you
must use the Xamarin Android Device Manager instead of the Google
Emulator Manager.

### [Google Emulator Manager](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

How to use the legacy Google Emulator Manager to create and customize
Google Android Emulator virtual devices. You can continue to run the
Google Android Emulator with the original Google Emulator Manager by
remaining on Android SDK Tools version 25.2.5 or lower.

After you have configured the Android SDK emulator, see
[Google Android Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
for information about how to launch the emulator and use it for testing
and debugging your app.
