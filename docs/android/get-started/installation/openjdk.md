---
title: "Preview with Microsoft's OpenJDK Distribution"
description: "A step-by-step guide to configuring Micorsoft's distribution of OpenJDK."
ms.prod: xamarin
ms.assetid: TBD
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/21/2018
---

# Preview with Microsoft's OpenJDK Distribution

_This guide describes the steps for switching to the preview release of Microsoft's distribution of OpenJDK._

## Overview

![New workflow offering a web preview of OpenJDK in VS 15.8 Preview 5](openjdk-images/openjdk.png)


With Visual Studio 15.9 and Visual Studio for Mac 7.7, we are moving from Oracle’s JDK to a lightweight version of Open JDK meant solely for Android development. The benefits of this move are:

- You will always have an OpenJDK version that works for Android development

- Downloading JDK 9 or 10 won’t affect development experience

- Significantly reduced download size and footprint

- No more issues with 3rd party servers and installers

If you’d like to move to the improve experience sooner, builds of our OpenJDK distribution are available for you to test on both Windows and Mac today. The setup is simple, and it is easy to revert back to the Oracle JDK at any time.

## Download

To get started, download the correct build for your system:

> [!IMPORTANT]
> TODO: set public links before publishing

- Mac: http://xamarin-jenkins/view/OpenJDK/job/openjdk-mac/20/Azure/processDownloadRequest/20/microsoft_dist_openjdk_1.8.0.7.zip
- Windows x86: http://xamarin-jenkins/view/OpenJDK/job/openjdk-win32/47/Azure/processDownloadRequest/47/microsoft_dist_openjdk_1.8.0.7.zip
- Windows x64: http://xamarin-jenkins/view/OpenJDK/job/openjdk-win64/21/Azure/processDownloadRequest/21/microsoft_dist_openjdk_1.8.0.7.zip

## Configure

Unzip to the correct location:

- Mac: `$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.8`
- Windows: `C:\Program Files\Android\jdk\microsoft_dist_openjdk_1.8.0.8`

> [!IMPORTANT]
> We are using build 1.8.0.8 in this example. The version you download may be newer.

Point the IDE to the new JDK:

- Mac: In `Tools > SDK Manager > Locations`, change the Java SDK (JDK) location to `$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.8`

![](openjdk-images/vsm.png)

- Windows: In `Tools > Options > Xamarin > Android Settings`, change the JDK location to `C:\Program Files\Android\jdk\microsoft_dist_openjdk_1.8.``0.``8`. 


![](openjdk-images/vs.png)

## Revert

To revert, point the IDE to the previous Oracle JDK location and rebuild the solution. On Mac, this can be accomplished by clicking “Reset to Defaults”.

If you encounter any issues with this distribution of OpenJDK, please report the issue using the feedback tool in your IDE so we can track and address it quickly.

## Known Issues & Planned Fix Dates

the `JAVA_HOME` environment variable may not be correctly exported to SDK and Device manager. As a workaround, you can set this environment variable yourself for the machine. This is fixed in the 15.9 Previews.

## Summary

In this article, you learned how to configure your IDE to use the preview release of Microsoft's OpenJDK distribution, which is slated for stable release in fall 2018. If you encounter any issues with this distribution of OpenJDK, please report the issue using the feedback tool in your IDE so we can track and address it quickly. 


