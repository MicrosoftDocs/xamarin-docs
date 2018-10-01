---
title: "Error MT1009: Could not copy the assembly"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
---

# Error MT1009: Could not copy the assembly

> [!IMPORTANT]
> This issue has been resolved in recent versions of Xamarin.iOS. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.

As described in our [Release Notes](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), this is a known issue in Xamarin.iOS 7.2.6. This issue is due to file permissions needing higher privileges when Xamarin.iOS is installed with a different user account then the developer's main account.

To workaround the issue, please open the Terminal.app on the Mac workstation and run the following command:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

