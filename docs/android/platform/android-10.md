---
title: "Android 10 features"
description: "How to get started developing apps for Android 10 using Xamarin.Android."
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
---
# Android 10 with Xamarin

_How to get started developing apps for Android 10 using Xamarin.Android._

Android 10 is now available from Google. A number of new features and APIs are being made available in this release, and many of them are necessary to take advantage of new hardware capabilities in the latest Android devices.

![Android 10 Logo](~/android/platform/android-10-images/android10_black.png)

This article is structured to help you get started in developing Xamarin.Android apps for Android 10. It explains how to install the necessary updates, configure the SDK, and prepare an emulator or device for testing. It also provides an outline of the new features in Android 10 and provides example source code that illustrates how to use some of the key Android 10 features.

Xamarin.Android 10.0 provides support for Android 10. For more information about Xamarin.Android support for Android 10, see the [Xamarin.Android 10.0 release notes](/xamarin/android/release-notes/10/10.0).

## Requirements

The following list is required to use Android 10 features in Xamarin-based apps:

- **Visual Studio** - Visual Studio 2019 is recommended. On Windows update to Visual Studio 2019 version 16.3 or later. On macOS, update to Visual Studio 2019 for Mac version 8.3 or later.
- **Xamarin.Android** - Xamarin.Android 10.0 or later must be installed with Visual Studio (Xamarin.Android is automatically installed as part of the **Mobile Development With .NET** workload on Windows and installed as part of the **Visual Studio for Mac Installer**)
- **Java Developer Kit** - Xamarin.Android 10.0 development requires JDK 8. Microsoft's distribution of the OpenJDK is automatically installed as part of Visual Studio.
- **Android SDK** - Android SDK API 29 must be installed via the Android SDK Manager.

## Get started

To get started developing Android 10 apps with Xamarin.Android, you must download and install the latest tools and SDK packages before you can create your first Android 10 project:

1. **Visual Studio 2019 is recommended**. Update to Visual Studio 2019 version 16.3 or later. If you are using Visual Studio for Mac 2019, update to Visual Studio 2019 for Mac version 8.3 or later.
2. Install **Android 10 (API 29)** packages and tools via the SDK Manager.
    - Android 10 (API 29) SDK Platform
    - Android 10 (API 29) System Image
    - Android SDK Build-Tools 29.0.0+
    - Android SDK Platform-Tools 29.0.0+
    - Android Emulator 29.0.0+
3. Create a new Xamarin.Android project that targets Android 10.0.
4. Configure an emulator or device for testing Android 10 apps.

Each of these steps is explained below:

### Update Visual Studio

Visual Studio 2019 is recommended for building Android 10 apps using Xamarin.

If you are using Visual Studio 2019, update to Visual Studio 2019 version 16.3 or later (for instructions, see [Update Visual Studio 2019 to the most recent release](/visualstudio/install/update-visual-studio)). On macOS, update to Visual Studio 2019 for Mac 8.3 or later (for instructions, see [Update Visual Studio 2019 for Mac to the most recent release](/visualstudio/mac/update)).

### Install the Android SDK

To create a project with Xamarin.Android 10.0, you must first use the Android SDK Manager to install the SDK platform for **Android 10 (API level 29)**.

1. Start the SDK Manager. In Visual Studio, click **Tools > Android > Android SDK Manager.** In Visual Studio for Mac, click **Tools > SDK Manager.**
2. In the lower right-hand corner, click the gear icon and select **Repository > Google (Unsupported):**

    ![Android SDK Manager Repository Selection](~/android/platform/android-10-images/sdkrepository.png)

3. Install the **Android 10 SDK Platform** packages, which are listed as **Android SDK Platform 29** in the **Platforms** tab (for more information about using the SDK Manager, see [Android SDK setup](../get-started/installation/android-sdk.md)):

    ![Android SDK Manager Platform Tab](~/android/platform/android-10-images/sdkplatforms.png)

### Create a Xamarin.Android project

Create a new Xamarin.Android project. If you are new to Android development with Xamarin, see [Hello, Android](../get-started/hello-android/index.md) to learn about creating Xamarin.Android projects.

When you create an Android project, you must configure the version settings to target Android 10.0 or later. For example, to target your project for Android 10, you must configure the target Android API level of your project to **Android 10.0 (API 29)**. This includes both your **Target Framework Version** and **Target Android SDK Version** to API 29 or later. For more information about configuring Android API levels, see [Understanding Android API Levels.](../app-fundamentals/android-api-levels.md)

![Xamarin.Android Target Framework](~/android/platform/android-10-images/targetframework.png)

### Configure a device or emulator

If you are using a physical device such as a Pixel, you can download the Android 10 update by going to the System > System update > Check for update in your phone's settings. If you'd prefer to flash your device, please see the instructions on flashing a [Factory Image](https://developers.google.com/android/images) or [OTA Image](https://developers.google.com/android/ota) to your device.

If you are using an emulator, create a virtual device for API level 29 and select an x86-based image. For information about using the Android Device Manager to create and manage virtual devices, see [Managing Virtual Devices with the Android Device Manager.](../get-started/installation/android-emulator/device-manager.md) For information about using the Android Emulator for testing and debugging, see [Debugging on the Android Emulator.](../deploy-test/debugging/debug-on-emulator.md)

## New features

Android 10 introduces a variety of new features. Some of these new features are intended to leverage new hardware capabilities offered by the latest Android devices, while others are designed to further enhance the Android user experience:

## Enhance your app with Android 10 features and APIs

Next, when you're ready, dive into Android 10 and learn about the [new features and APIs](https://developer.android.com/preview/api-overview.html) that you can use. Here are some of the top features to get started with.

These features are recommend for every app:

- **Dark Theme:** Ensure a consistent experience for users who enable system-wide dark theme by adding a [Dark Theme](https://developer.android.com/preview/features/darktheme) or enabling [Force Dark](https://developer.android.com/preview/features/darktheme#force_dark).

![Dark Theme](~/android/platform/android-10-images/darktheme.png)

- **Support [gestural navigation](https://developer.android.com/preview/features/gesturalnav)** in your app by going edge-to-edge and making sure your custom gestures are complementary to the system navigation gestures.

![Gesture Navigation](~/android/platform/android-10-images/gesturenavigation.png)

- **Optimize for foldables:** Deliver seamless, edge-to-edge experiences on today’s innovative devices by [optimizing for foldables](https://developer.android.com/guide/topics/ui/foldables).

![Foldable](~/android/platform/android-10-images/foldable.png)

These features are recommended if relevant for your app:

- **More interactive notifications:** If your notifications include messages, enable [suggested replies and actions in notifications](https://developer.android.com/preview/features#smart-suggestions) to engage users and let them take action instantly.
- **Better biometrics:** If you use biometric auth, move to [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt), the preferred way to support fingerprint auth on modern devices.
- **Enriched recording:** To support captioning or gameplay recording, enable [audio playback capture](https://developer.android.com/preview/features/playback-capture). It’s a great way to reach more users and make your app more accessible.
- **Better codecs:** For media apps, try [AV1](https://en.wikipedia.org/wiki/AV1) for video streaming and [HDR10+](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) for high dynamic range video. For speech and music streaming, you can use [Opus](http://opus-codec.org/) encoding, and for musicians, a [native MIDI API](https://developer.android.com/preview/features/midi) is available.
- **Better networking APIs:** If your app manages IoT devices over Wi-Fi, try the new [network connection APIs](https://developer.android.com/preview/features#peer2peer) for functions like configuring, downloading, or printing.

These are just a few of the many new features and APIs in Android 10. To see them all, visit the [Android 10 site for developers](https://developer.android.com/about/versions/10/highlights).

## Behavior changes

When the Target Android Version is set to API level 29, there are several platform changes that cann affect your app's behavior even if you are not implementing the new features described above. The following list is a brief summary of these changes:

- [To ensure app stability and compatibility, the Android platform now restricts non-SDK interfaces your app can use in Android 10](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions).
- [Shared memory has changed](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Android runtime & AOT correctness](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- [Permissions for fullscreen intents must request `USE_FULL_SCREEN_INTENT`](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Support for foldables](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## Summary

This article introduced Android 10 and explained how to install and configure the latest tools and packages for Xamarin.Android development with Android 10. It provided an overview of the key features available in Android 10. It included links to API documentation and Android Developer topics to help you get started in creating apps for Android 10. It also highlighted the most important Android 10 behavior changes that could impact existing apps.

## Related links

- [Android 10](https://developer.android.com/about/versions/10)
