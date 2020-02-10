---
title: "Which Android SDK packages should I install?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
---

# Which Android SDK packages should I install?

Installing the Android SDK doesn't automatically include all the minimum required packages for developing. While individual developer needs vary, the following packages will generally be required for developing with Xamarin.Android:

## Tools

Install the latest tools from the Tools folder in the SDK manager:

- Android SDK Tools
- Android SDK Platform-Tools
- Android SDK Build-Tools

## Android Platform(s)

Install the "SDK Platform" for the Android versions you've set as minimum & target.

Examples:

- Target API 23
- Minimum API 23

Only need to install SDK Platform for API 23

- Target API 23
- Minimum API 15

Need to install SDK Platforms for API 15 and 23. Note that you do not need
to install the API levels between the minimum and target (even if you
are backporting to those API levels).

## System Images

These are only required if you want to use the out-of-the-box Android
emulators from Google. For more information, see
[Android Emulator Setup](~/android/get-started/installation/android-emulator/index.md)

## Extras
The Android SDK Extras are usually not required; but it is useful to be aware of them since they may be required depending on your use case.
