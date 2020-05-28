---
title: "Android build error – The LinkAssemblies task failed unexpectedly"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Android build error – The LinkAssemblies task failed unexpectedly

You may see an error message `The "LinkAssemblies" task failed unexpectedly` when building a Xamarin.Android project that uses Forms. This happens when the linker is active (typically on a *Release* build to reduce the size of the app package); and it occurs because the Android targets aren't updated to the latest framework. (More information: [Xamarin.Forms supported platforms](~/get-started/supported-platforms.md#android-platform-support))

The resolution to this issue is to make sure you have the latest supported Android SDK versions, and set the **Target Framework** to the latest installed platform. It's also recommended that you set the **Target Android Version** to the latest installed platform, and the **minimum Android version** to API 19 or higher. This is considered the supported configuration.

## Setting in Visual Studio for Mac

1. Right click on the Android project, and select **Options** in the menu.
2. In the **Project Options** dialog, go to **Build > General**.
3. Set the **Compile using Android version: (Target Framework)** to the latest installed platform.
4. In the **Project Options** dialog, go to **Build > Android Application**.
5. Set the **Minimum Android version** to API level 19 or higher, and the **Target Android version** to the latest installed platform you chose in (3).

## Setting in Visual Studio

1. Right click on the Android project, and select **Properies** in the menu.
2. In the project properties, go to **Application**.
3. Set the **Compile using Android version: (Target Framework)** to the latest installed platform.
4. In the project properties, go to **Android Manifest**.
5. Set the **Minimum Android version** to API level 19 or higher, and the **Target Android version** to the latest installed platform you chose in (3).

Once you've updated those settings, please clean and rebuild your project to ensure your changes are picked up.
