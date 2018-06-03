---
title: "Google Android Emulator Setup"
description: "The Google Android Emulator can be run in a variety of configurations to simulate different devices. This guide explain how to prepare the Android Emulator for testing your app."
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
---

# Google Android Emulator Setup

_This guide explain how to prepare the Google Android Emulator for testing your app._


## Overview

The Google Android Emulator can be run in a variety of configurations
to simulate different devices. Each configuration is called a _virtual
device_. When you deploy and test your app on the emulator, you select
a pre-configured or custom virtual device that simulates a physical
Android device such as a Nexus or Pixel phone.

The sections listed below describe how to accelerate the Google Android
emulator for maximum performance, how to use the Android Device Manager
to create and customize virtual devices, and how to customize the
profile properties of a virtual device. In addition, a troubleshooting
section explains common setup problems and workarounds.

## Sections

### [Hardware Acceleration for Emulator Performance](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

How to prepare your computer for maximum Android Emulator performance.
Because the Google Android Emulator can be prohibitively slow without
hardware acceleration, we recommend that you enable hardware
acceleration on your computer before you use this emulator.

### [Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)

How to use the Android Device Manager to create and customize virtual
devices.

### [Editing Android Virtual Device Properties](~/android/get-started/installation/android-emulator/device-properties.md)

How to use the Android Device Manager to edit the profile properties of an
Android virtual device.

### [Troubleshooting Emulator Setup Problems](~/android/get-started/installation/android-emulator/troubleshooting.md)

How to diagnose and correct Android Device Manager problems that may
occur when setting up the Android Emulator.


After you have configured the Android Emulator, see
[Debugging with the Google Android Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
for information about how to launch the emulator and use it for testing
and debugging your app.


> [!NOTE]
> As of Android SDK Tools version **26.0.1** and later, Google has removed support for existing AVD/SDK managers in favor of their new CLI (Command Line Interface) tools. Because of this deprecation change, Xamarin SDK/Device Managers are now used instead of Google SDK/Device Managers for Android Tools 26.0.1 and later. For more information about the Xamarin SDK Manager, see [Android SDK Setup](~/android/get-started/installation/android-sdk.md).

