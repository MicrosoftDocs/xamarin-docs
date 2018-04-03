---
title: "Limitations"
description: "Some limitations of the Xamarin Live Player"
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
---

# Limitations

![Preview feature](~/media/shared/preview.png)

## Device Requirements
The Xamarin Live Player app supports the following devices:

# [Android](#tab/android)

- Android 4.2 or later.
- ARM-v7a, ARM-v8a, ARM64-v8a, x86, or x86_64 processor.

# [iOS](#tab/ios)

- iOS 9.0 or later.
- ARM64 processor.

-----

## Limitations

There are some limitations on the things Xamarin Live Player can run, including the items below:

### Xamarin.Forms
- Custom Renderers are not supported.
- Effects are not supported.
- Custom Controls with Custom Bindable Properties are not supported.
- Embedded resources are not supported (ie. embedding images or other resources in a PCL).
- Third party MVVM frameworks are not supported (ie. Prism, Mvvm Cross, Mvvm Light, etc.).
- Asset Catalogs on iOS are not supported.

### Other project types
- Live Player is not intended for native Android or iOS projects (that use Android XML or Storyboards for the user interface).

### Misc
- Limited support for reflection (currently affects some popular NuGets, like SQLite and Json.NET). Other NuGets may still be supported.
- Some system classes cannot be overridden (for example, you cannot implement a subclass).
- Some platform features that require provisioning can't work in the Xamarin Live Player app (however it has been configured for common operations like photo gallery access).
- Custom targets and build steps are ignored. For example, tools like Fody, Retit, AutoFac, and AutoMapper cannot be incorporated.
- F# projects are not supported on Android and limited support on iOS
- Advanced scenarios with custom generic classes and interfaces may not be supported.

Please report any additional issues on [bugzilla](https://aka.ms/live-player-report-issue).


## Related Links

- [Troubleshooting](~/tools/live-player/troubleshooting.md)
- [Setup](~/tools/live-player/install.md)
