---
title: "Xamarin.Android and JDK 9"
description: "This article explains how to resolve Java Development Kit (JDK) 9 errors in Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# Xamarin.Android and JDK 9

_This article explains how to resolve Java Development Kit (JDK) 9 errors in Xamarin.Android._


## Overview

Xamarin.Android uses the Java Development Kit (JDK) to integrate with
the Android SDK for building Android apps and running the Android
designer. The latest versions of the Android SDK (API 24 and higher)
require JDK 8 (1.8). **Because the Android SDK tools available from
Google are not yet compatible with JDK 9, Xamarin.Android does not work
with JDK 9.**

## JDK 9 Errors

If you try to build a Xamarin.Android project with JDK 9 installed, you
will get an explicit error indicating that JDK 9 is not supported.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors	
```

To resolve these errors, you must install JDK 8 (1.8) as explained in
[How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md).


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

If JDK 9 is installed, you must install Java JDK 8 (1.8). For
information about how to install JDK 8, see
[How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md)

## Known Issues with JDK 9

### apksigner

There is a known issue with apksigner and JDK 9 in which the `apksigner.bat` file invokes the `apksigner.jar` with `-Djava.ext.dirs` instead of `-classpath` which JDK 9 expects. It is recommended to use JDK 8 (1.8). For information about how to install JDK 8, see [How do I update the Java Development Kit (JDK) version?](~/android/troubleshooting/questions/update-jdk.md)

After uninstalling JDK 9, ensure that the following path is not set on your `PATH` environment variable as it will still point to JDK 9: `C:\ProgramData\Oracle\Java\javapath`. After removing it, `java -version` at a command line should show JDK 8.
