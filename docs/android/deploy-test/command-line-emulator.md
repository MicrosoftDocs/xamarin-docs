---
title: "Command Line Emulator"
ms.service: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/05/2018
description: Learn how to run the Android emulator from the command line by using the emulator tool provided by the Android SDK.
---

# Command Line Emulator

## Running the Android emulator from the command line

To enable running the Android emulator from the command line, you can use the
"emulator" tool provided by the Android SDK. This tool can be used to run the
emulator from Terminal on OS X or from Command Prompt on a Windows machine.

To launch a specific Android emulator, run the following command from the
tools directory in the android SDK location (such as
C:\android-sdk-windows\tools):

On Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

On macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

The reason for needing the partition size is to allow the emulator to have
plenty of space to get the Xamarin.Android platform installed on the emulator
as by default the size of the emulator is small.

You can find out more information on extra parameters on the Android site
here - [https://developer.android.com/studio/run/emulator-commandline](https://developer.android.com/studio/run/emulator-commandline)
