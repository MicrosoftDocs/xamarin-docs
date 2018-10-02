---
title: "Xamarin.Android and Java Development Kit 9"
description: "This article explains how to resolve Java Development Kit (JDK) 9 or later errors in Xamarin.Android."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
---

# Xamarin.Android and Java Development Kit 9 or later

_This article explains how to resolve Java Development Kit (JDK) 9 or
later errors in Xamarin.Android._


## Overview

Xamarin.Android uses the Java Development Kit (JDK) to integrate with
the Android SDK for building Android apps and running the Android
designer. The latest versions of the Android SDK (API 24 and higher)
require JDK 8 (1.8) or the Microsoft Mobile OpenJDK Preview. **Because
the Android SDK tools available from Google are not yet compatible with
JDK 9, Xamarin.Android does not work with JDK 9 or later.**

## JDK Errors

If you try to build a Xamarin.Android project with a version of the
JDK later than JDK 8, you will get an explicit error indicating that
this version of JDK is not supported. For example:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors	
```

To resolve these errors, you must install JDK 8 (1.8) as explained in
[How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md).
Alternately, you can install the 
[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) 
The Microsoft Mobile OpenJDK will eventually replace JDK 8 for Xamarin.Android development.


## Checking the JDK Version

You can check to see which version of Java you have installed
by entering the following command (the JDK `bin` directory must
be in your `PATH`):

```shell
java -version
```

If JDK 9 installed, you will see a message like the following:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

If JDK 9 or later is installed, you must install Java JDK 8 (1.8) or
the Microsoft Mobile OpenJDK Preview. For information about how to
install JDK 8, see
[How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md). 
For information about how to install the Microsoft Mobile OpenJDK, see
[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md).

Note that you do not have to uninstall a later version of the JDK;
however, you must ensure that Xamarin is using JDK 8 rather than a
later JDK version. In Visual Studio, click 
**Tools > Options > Xamarin > Android Settings**. 
If **Java Development Kit Location** is not set
to a JDK 8 location (such as **C:\\Program Files\\Java\\jdk1.8.0_111**), 
click **Change** and set it to the location where JDK 8 is installed. 
In Visual Studio for Mac, navigate
to **Preferences > Projects > SDK Locations > Android > Java SDK
(JDK)** and click **Browse** to update this path.

## Known Issues with JDK 9

### apksigner

There is a known issue with apksigner and JDK 9 in which the
`apksigner.bat` file invokes the `apksigner.jar` with `-Djava.ext.dirs`
instead of `-classpath` which JDK 9 expects. It is recommended to use
JDK 8 (1.8). For information about how to install JDK 8, see
[How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md)

If you have installed JDK 9, ensure that the following path is not set on
your `PATH` environment variable as it will still point to JDK 9:
`C:\ProgramData\Oracle\Java\javapath`. After removing it, `java-version` at a 
command line should show JDK 8.
