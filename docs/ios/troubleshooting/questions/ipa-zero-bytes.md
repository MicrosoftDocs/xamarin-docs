---
title: "IPA file is 0 bytes"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
---

# IPA file is 0 bytes

> [!IMPORTANT]
> This issue has been resolved in recent versions of Xamarin. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.



There were some known issues in previous versions of Xamarin that could cause the IPA file on Windows to be 0 bytes. 

### Fixed in Xamarin for Visual Studio 3.11.584 
- [Bug 24416 - Building "Ad-Hoc" configuration from command line does not copy IPA file to Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Bug 24417 - Changing "Project Properties -> iOS IPA Options -> Package name" prevents IPA from being copied back to Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [Bug 29822 - [XVS.iOS 3.11] Setting "Build" number different from "Version" number causes IPA not to be copied to Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### Fixed in Xamarin for Visual Studio 4.1.0.496
- [Bug 27989 - Show ipa file on Build server fails if the Assembly name does not match the Project name](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
