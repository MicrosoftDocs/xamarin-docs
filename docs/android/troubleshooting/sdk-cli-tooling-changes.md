---
title: "Changes to the Android SDK Tooling"
description: "Changes to how the Android SDK manages the installed API levels and AVDs."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
---

# Changes to the Android SDK Tooling

_Changes to how the Android SDK manages the installed API levels and AVDs._

## Changes to Android SDK Tooling

In recent versions of the SDK Tools for Android, Google has removed the
existing AVD and SDK managers in favor of new CLI (Command Line
Interface) tooling. The **android** program has been removed and the
Google GUI (Graphical User Interface) managers in Visual Studio for Mac
and older versions of Visual Studio Tools for Xamarin will no longer work
past version 25.2.5 of Android SDK Tools. For example, attempting to
use the **android** program via the command line will result in an
error message like the following:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

The following sections explain how to manage the Android SDK and
Android Virtual Devices using Android SDK 25.3.0 and later.

### UI Tools

Visual Studio and Visual Studio for Mac now provide Xamarin replacements
for the discontinued Google GUI-based managers:

-   To download Android SDK tools, platforms, and other components that
    you need for developing Xamarin.Android apps, use the
    [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md)
    instead of the legacy Google SDK Manager.

-   To create and configure Android Virtual Devices, use 
    the [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
    instead of the legacy Google Emulator Manager.

These tools are functionally equivalent to the Google GUI-based
managers they replace.

### CLI Tools

Alternately, you can use CLI tools to manage and update your emulators
and Android SDK. The following programs now make up the command line
interface for the Android SDK tools:

#### sdkmanager

**Added In:** Android SDK Tools 25.2.3 (November, 2016) and higher.

There is a new program called **sdkmanager** in the **tools/bin**
folder of your Android SDK. This tool is used to maintain the Android
SDK at the command line. For more information about using this tool,
see [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### avdmanager

**Added In:** Android SDK Tools 25.3.0 (March, 2017) and higher.

There is a new program called **avdmanager** in the **tools/bin**
folder of your Android SDK. This tool is used to maintain the AVDs for
the Android Emulator. For more information about using this
tool, see [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### Downgrading

You can downgrade your **Android SDK Tools** version by installing a
previous version of the Android SDK from the
[Android Developer website](https://developer.android.com/studio/index.html).

### Using the old GUI

You can still use the original GUI by running the **android** program
inside your **tools** folder as long as you are on **Android SDK
Tools** version **25.2.5** or lower.


## Related Links

- [Android SDK Setup](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Understanding Android API levels](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools Release Notes (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
