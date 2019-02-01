---
title: "Android build error – The LinkAssemblies task failed unexpectedly"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
---

# Android build error – The LinkAssemblies task failed unexpectedly

You may see an error message `The "LinkAssemblies" task failed unexpectedly` when building a Xamarin.Android project that uses Forms. This happens when the linker is active (typically on a *Release* build to reduce the size of the app package); and it occurs because the Android targets aren't updated to the latest framework. (More information: [Xamarin.Forms for Android Requirements](~/get-started/installation.md#android))

The resolution to this issue is to make sure you have the latest supported Android SDK versions, and set the **Target Framework** to **Use latest installed platform**. It's also recommended that you set the **Target Android Version** to **Use Target Framework Version** and the **minimum Android version** to API 15 or higher. This is considered the supported configuration.

## Setting in Visual Studio for Mac

1.  Right click on the Android project.
2.  Go to **Build > General > Target Framework**.
3.  Set the **Target Framework: Use latest installed platform**.
4.  Still in the project options, go to **Build > Android Application**.
5.  Set the **Minimum Android version** to API level 15 or higher & the **Target Android version** to **Automatic - use target framework version**.

## Setting in Visual Studio

1.  Right click on the Android project.
2.  Go to **Application** in the project options.
3.  Set the **Compile using Android version** & **Target Android version** settings to **Use Latest Platform** / **Use Compile using SDK version**.
4.  Set the **Minimum Android to target** setting to API 19 or higher.

Once you've updated those settings, please clean and rebuild your project to ensure your changes are picked up.
