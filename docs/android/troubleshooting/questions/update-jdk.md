---
title: "How do I update the Java Development Kit (JDK) version?"
description: "This article illustrates how to update the Java Development Kit (JDK) version on Windows and Mac."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
---

# How do I update the Java Development Kit (JDK) version?

_This article illustrates how to update the Java Development Kit (JDK) version on Windows and Mac._

## Overview

Xamarin.Android uses the Java Development Kit (JDK) to integrate with
the Android SDK for building Android apps and running the Android
designer. The latest versions of the Android SDK (API 24 and higher)
require JDK 8 (1.8). Alternately, you can install the
[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). 
The Microsoft Mobile OpenJDK will eventually replace JDK 8 for Xamarin.Android
development.

To update to the Microsoft Mobile OpenJDK, see
[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). 
To update to JDK 8, follow these steps:

# [Visual Studio](#tab/windows)

1.  Download JDK 8 (1.8) from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot of the JDK download page on the Oracle website](update-jdk-images/image1.png)

2.  Pick the 64-bit version to allow rendering of 
    [custom controls](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols)
    in the Xamarin Android designer:

    ![Selecting the Windows x64 JDK package to download from the JDK download page](update-jdk-images/image2.png)

3.  Run the .exe and install the **Development Tools**:

    ![Installing the Development Tools in the JDK installer](update-jdk-images/image3.png)

4.  Open Visual Studio and update the **Java Development Kit Location**
    to point to the new JDK under **Tools > Options > Xamarin > Android
    Settings > Java Development Kit Location**:

    [![Path setting for the JDK in the Android Settings page](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Be sure to restart Visual Studio after updating the location.

# [Visual Studio for Mac](#tab/macos)

1.  Download JDK 8 (1.8) from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot of the JDK download page on the Oracle website](update-jdk-images/image1.png)

2.  Open the .dmg file and run the .pkg installer:

    ![Running the JDK installer on macOS](update-jdk-images/image5.png)

Mac OS will automatically set the new JDK version as the default by
updating **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. 
You can then double-check that the **Java SDK (JDK)** location is set to
the expected default of **/usr** under **Visual Studio for Mac > Preferences >
Projects > SDK Locations > Android > Locations > Java SDK (JDK) Location**:

[![Setting the JDK location in the Android Locations tab](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

