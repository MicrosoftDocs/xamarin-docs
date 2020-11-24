---
title: "Xamarin.Forms supported platforms"
description: "Platform and development system requirements for Xamarin.Forms."
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/13/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms supported platforms

Xamarin.Forms applications can be written for the following operating systems:

- iOS 9 or higher.
- Android 4.4 (API 19) or higher ([more details](#android-platform-support)). However, Android 5.0 (API 21) is recommended as the minimum API. This ensures full compatibility with all the Android support libraries, while still targeting the majority of Android devices.
- Windows 10 Universal Windows Platform, build 10.0.16299.0 or greater for .NET Standard 2.0 support. However, build 10.0.18362.0 or greater is recommended.

Xamarin.Forms apps for iOS, Android, and the Universal Windows Platform (UWP) can be built in Visual Studio. However, a networked Mac is required for iOS development using the latest version of Xcode and the minimum version of macOS specified by Apple. For more information, see [Windows requirements](~/cross-platform/get-started/requirements.md#windows-requirements).

Xamarin.Forms apps for iOS and Android can be built in Visual Studio for Mac. For more information, see [macOS requirements](~/cross-platform/get-started/requirements.md#macos-requirements).

> [!NOTE]
> Developing apps using Xamarin.Forms requires familiarity with [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## Additional platform support

Xamarin.Forms supports additional platforms beyond iOS, Android, and Windows:

- Samsung Tizen
- macOS 10.13 or higher
- GTK#
- WPF

The status of these platforms is available on the [Xamarin.Forms GitHub platform support wiki](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

## Android platform support

You should have the latest Android SDK Tools and Android API platform installed. You can update to the latest versions using the [Android SDK Manager](~/android/get-started/installation/android-sdk.md).

Additionally, the target/compile version for Android projects **must** be set to *Use latest installed platform*. However the minimum version can be set to API 19 so you can continue to support devices that use Android 4.4 and newer. These values are set in the **Project Options**:

# [Visual Studio](#tab/windows)

**Project Options > Application > Application Properties**

![Android build options in Visual Studio](requirements-images/options-android-vs-sml.png)

# [Visual Studio for Mac](#tab/macos)

**Build > General**

![Select the latest target framework](requirements-images/options-general-sml.png)

**Build > Android Application**

![Select the minimum and target Android versions for your app](requirements-images/options-android-sml.png)

-----

## Deprecated platforms

These platforms are not supported when using Xamarin.Forms 3.0 or newer:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
