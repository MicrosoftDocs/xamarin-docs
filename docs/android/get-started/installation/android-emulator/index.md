---
title: "Android Emulator Setup"
description: "The Android Emulator can be run in a variety of configurations to simulate different devices. This guide explains how to prepare the Android Emulator for testing your app."
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/27/2018
---

# Android Emulator Setup

_This guide explains how to prepare the Android Emulator for testing your app._


## Overview

The Android Emulator can be run in a variety of configurations
to simulate different devices. Each configuration is called a _virtual
device_. When you deploy and test your app on the emulator, you select
a pre-configured or custom virtual device that simulates a physical
Android device such as a Nexus or Pixel phone.

The sections listed below describe how to accelerate the Android
emulator for maximum performance, how to use the Android Device Manager
to create and customize virtual devices, and how to customize the
profile properties of a virtual device. In addition, a troubleshooting
section explains common emulator problems and workarounds.

## Sections

### [Hardware Acceleration for Emulator Performance](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

How to prepare your computer for maximum Android Emulator performance
by using either Hyper-V or HAXM virtualization technology. Because the
Android Emulator can be prohibitively slow without hardware
acceleration, we recommend that you enable hardware acceleration on
your computer before you use the emulator.

### [Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)

How to use the Android Device Manager to create and customize virtual
devices.

### [Editing Android Virtual Device Properties](~/android/get-started/installation/android-emulator/device-properties.md)

How to use the Android Device Manager to edit the profile properties of a
virtual device.

### [Android Emulator Troubleshooting](~/android/get-started/installation/android-emulator/troubleshooting.md)

In this article, the most common warning messages and issues that occur
while running the Android Emulator are described, along with
workarounds and tips.

After you have configured the Android Emulator, see
[Debugging on the Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)
for information about how to launch the emulator and use it for testing
and debugging your app.


> [!NOTE]
> As of Android SDK Tools version **26.0.1** and later, Google has removed support for existing AVD/SDK managers in favor of their new CLI (Command Line Interface) tools. Because of this deprecation change, Xamarin SDK/Device Managers are now used instead of Google SDK/Device Managers for Android Tools 26.0.1 and later. For more information about the Xamarin SDK Manager, see [Setting up the Android SDK for Xamarin.Android](~/android/get-started/installation/android-sdk.md).

