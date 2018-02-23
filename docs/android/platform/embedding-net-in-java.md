---
title: "Embedding .NET in Java"
description: "How to use a Xamarin .NET library in a Java-based Native Android Project"
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
---

# Embedding .NET in Java

In some cases, you may want to add a Xamarin .NET library to an
existing native Android project. To do this, you can use the
[Embeddinator-4000](https://mono.github.io/Embeddinator-4000/) tool to
turn your .NET library into a native library that can be incorporated
into a native Java-based Android app.

 
## Requirements

To use the Embeddinator-4000 with Java on Android, you will need the
following:

-   **Android Studio** &ndash;
    [Android Studio
    3.x](https://developer.android.com/studio/preview/index.html) or
    later must be installed.

-   **Xamarin.Android** &ndash;
    [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/)
    or later must be installed.

-   **Java Developer Kit** &ndash;
    [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    or later must be installed.

-   **Mono** &ndash;
    [Mono 5.0](http://www.mono-project.com/download/) or later must be
    installed.


# [Visual Studio](#tab/vswin)

You can use Visual Studio to edit and compile your C# code.

# [Visual Studio for Mac](#tab/vsmac)

You can use Visual Studio for Mac to edit and compile your C# code.

-----

 
## Using the Embeddinator-4000

To consume a .NET library in a native Android project, you use the
following steps:

1.  Create a C# Android Library project.

2.  Install Embeddinator-4000 via NuGet.

3.  Run Embeddinator on the Android library assembly.

4.  Use the generated AAR file in a Java project in Android Studio.

These steps are described in detail in the 
[Embeddinator-4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html)
documentation.
