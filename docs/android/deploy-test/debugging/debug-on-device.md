---
title: "Debug on Device"
description: "This article explains how to debug a Xamarin.Android application on a physical Android device."
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# Debug on Device

_This article explains how to debug a Xamarin.Android application on a physical Android device._

## Debug on Device Overview

It is possible to debug a Xamarin.Android app on an Android device using
either Visual Studio for Mac or Visual Studio. Before debugging can
occur on a device, it must be
[setup for development](~/android/get-started/installation/set-up-device-for-development.md)
and connected to your PC or Mac.


## Debug Application

Once a device is connected to your computer, debugging a 
Xamarin.Android application is done in the same way as any other 
Xamarin product or .NET application. Ensure that the **Debug** 
configuration and the external device is selected in the IDE, this will 
ensure that the necessary debug symbols are available and that the IDE 
can connect to the running application: 

# [Visual Studio](#tab/windows)

![Debug configuration selected](debug-on-device-images/image1-vs.png)

Next, a breakpoint is set in the code:

![Breakpoint set at line of code](debug-on-device-images/image2-vs.png)

Once the device has been selected, Xamarin.Android will connect to the 
device, deploy the application, and then run it. When the breakpoint is 
reached, the debugger will stop the application, allowing the 
application to be debugged in a fashion similar to any other C# 
application: 

![Breakpoint reached](debug-on-device-images/image3-vs.png)

# [Visual Studio for Mac](#tab/macos)

![Debug configuration selected](debug-on-device-images/image1-xs.png)

Next, a breakpoint is set in the code:

![Breakpoint set at line of code](debug-on-device-images/image2-xs.png)

Once the device has been selected, Xamarin.Android will connect to the 
device, deploy the application, and then run it. When the breakpoint is 
reached, the debugger will stop the application, allowing the 
application to be debugged in a fashion similar to any other C# 
application: 

![Breakpoint reached](debug-on-device-images/image3-xs.png)

-----



## Summary

In this document discussed how to debug a Xamarin.Android
application by setting a breakpoint and selecting the target
device.


## Related Links

- [Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)
- [Setting the Debuggable Attribute](~/android/deploy-test/debuggable-attribute.md)
