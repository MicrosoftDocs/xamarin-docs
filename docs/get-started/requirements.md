---
title: "Xamarin.Forms Requirements"
description: "Platform and development system requirements for Xamarin.Forms."
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
---
# Xamarin.Forms Requirements

_Platform and development system requirements for Xamarin.Forms._

Refer to the [Installation](installation/index.md) article for an overview of installation and setup practices that apply across platforms.

## Target platforms

Xamarin.Forms applications can be written for the following operating systems:

- iOS 9 or higher
- Android 4.4 (API 19) or higher ([more details](#android))
- Windows 10 Universal Windows Platform ([more details](#windows10))

However, Android 5.0 (API 21) is recommended as the minimum API. This ensures full compatibility with all the Android support libraries, while still targeting the majority of Android devices.

It is assumed that developers have familiarity with [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

### Additional platform support

The status of these platforms is available on the [Xamarin.Forms GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- macOS
- GTK#
- WPF

### Android

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

## Development system requirements

Xamarin.Forms apps can be developed on macOS and Windows. However, Windows and Visual Studio are required to produce Windows versions of the app.

## Mac System requirements

You can use Visual Studio for Mac to develop Xamarin.Forms apps on macOS High Sierra (10.13) or newer. To develop iOS apps, we recommend using the latest version of Xcode, iOS, and macOS. For specific version requirements, refer to the latest [Xamarin.iOS release notes](/xamarin/ios/release-notes/).

> [!NOTE]
> Windows apps cannot be developed on macOS.

## Windows system requirements

Xamarin.Forms apps for iOS and Android can be built on any Windows installation that supports Xamarin development. For full support of the current platform features, use the latest version of Visual Studio. 

A networked Mac is required for iOS development using the latest version of Xcode and the minimum version of macOS specified by Apple.

<a name="windows10" />

### Universal Windows Platform (UWP)

Developing Xamarin.Forms apps for UWP requires:

- Windows 10 (latest version recommended, Fall Creators Update minimum)

- Visual Studio 2019 recommended

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

You can [add a Universal Windows Platform (UWP) App](~/xamarin-forms/platform/windows/installation/index.md) to an existing
Xamarin.Forms solution at any time.

## Deprecated platforms

These platforms are not supported when using Xamarin.Forms 3.0 or newer:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
