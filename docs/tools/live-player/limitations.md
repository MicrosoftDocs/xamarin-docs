---
title: "Limitations of Xamarin Live Player"
description: "This document describes limitations of the Xamarin Live Player. It discusses device requirements, features it works with, project types, and other miscellaneous topics."
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
---

# Limitations of Xamarin Live Player

![Preview feature](~/media/shared/preview.png)

## Device Requirements

The Xamarin Live Player app supports the following Android devices:

- Android 4.2 or later.
- ARM-v7a, ARM-v8a, ARM64-v8a, x86, or x86_64 processor.

## Limitations

There are some limitations on the things Xamarin Live Player can run, including the items below:

### iOS

Live Player is not available for iOS.

### Xamarin.Forms

- Custom Renderers are not supported.
- Effects are not supported.
- Custom Controls with Custom Bindable Properties are not supported.
- Embedded resources are not supported (ie. embedding images or other resources in a PCL).
- Third party MVVM frameworks are not supported (ie. Prism, Mvvm Cross, Mvvm Light, etc.).

### Other project types

- Live Player is not intended for native Android projects (that use Android XML for the user interface).

### Misc

- Limited support for reflection (currently affects some popular NuGets, like SQLite and Json.NET). Other NuGets may still be supported.
- Some system classes cannot be overridden (for example, you cannot implement a subclass).
- Some platform features that require provisioning can't work in the Xamarin Live Player app (however it has been configured for common operations like photo gallery access).
- Custom targets and build steps are ignored. For example, tools like Fody, Refit, AutoFac, and AutoMapper cannot be incorporated.
- F# projects are not supported
- Advanced scenarios with custom generic classes and interfaces may not be supported.

Use **Report a Problem** in [Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)
or [Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem)
to report any issues with Xamarin Live Player.

## Related Links

- [Troubleshooting](~/tools/live-player/troubleshooting.md)
- [Setup](~/tools/live-player/install.md)
