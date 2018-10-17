---
title: "What version of Xamarin.Android added Lollipop support?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# What version of Xamarin.Android added Lollipop support?

**Note:** This guide was originally written for the Android L preview.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) added Android L Preview support.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) added Android Lollipop support.

Xamarin only actively supports the current stable release of the
Xamarin tools. The information below is provided "as-is" for older
versions of the tools. For the latest information on Xamarin releases,
please check [here](http://releases.xamarin.com/).

## "Missing android.jar for API Level 21" in Android L Preview

# [Visual Studio](#tab/windows)

The following error message (or similar) may show up:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

This message means that the Android SDK platform for API Level 21 is
not installed. Either install it in the Android SDK Manager (Tools >
Open Android SDK Manager...), or change your Xamarin.Android project to
target an API version that is installed.

There are a few workarounds for this issue:

1. Change your project so that it targets API 19 or lower.

2. Rename your android-21 folder from android-21 to android-L. (At
   best, this should only be used as a temporary fix, and it might not
   work very well at all.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Temporarily downgrade back to the Android API Level 21 "L" preview [1]:

    1.  Delete the **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21** 
    2.  Extract [1] into **C:\\Users\\<username>\\AppData\\Local\\Android\\android-sdk\\platforms** 
        to create an **android-L** folder.

# [Visual Studio for Mac](#tab/macos)

The following error message (or similar) may show up:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

This means that the Android SDK platform for API Level 21 is not
installed.Either install it in the Android SDK Manager (Tools > SDK
Manager...), or change your Xamarin.Android project to target an API
version that is installed.

There are a few workarounds for this issue:

1. Change your project so that it targets API 19 or lower.

2. Rename your android-21 folder from android-21 to android-L. (At
   best, this should only be used as a temporary fix, and it might not
   work very well at all.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Temporarily downgrade back to the Android API Level 21 "L" preview [1]:

    1.  Delete **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Extract [1] into **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**
        to create an **android-L** folder.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
