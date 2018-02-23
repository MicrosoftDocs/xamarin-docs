---
title: "Limitations"
description: "Some limitations of the Xamarin Live Player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
---

# Limitations

![Preview feature](~/media/shared/preview.png)

## Device Requirements
The Xamarin Live Player app supports the following devices:

### iOS
- iOS 9.0 or later.
- ARM64 processor.
- Check the [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) for a list of supported devices.

### Android
- Android 4.2 or later.
- ARM-v7a, ARM-v8a, ARM64-v8a, x86, or x86_64 processor.


## Limitations

There are some limitations on the things Xamarin Live Player can run, including the items below:

### Android
- Android user interfaces designed with AXML files are not currently supported.

### iOS
- Some iOS storyboard features are not supported.
- iOS XIB files are not supported.

### Xamarin.Forms
- Custom Renderers are not supported.
- Effects are not supported.
- Custom Controls with Custom Bindable Properties are not supported.
- Embedded resources are not supported (ie. embedding images or other resources in a PCL).

### Misc
- Limited support for reflection (currently affects some popular NuGets, like SQLite and Json.NET). Other NuGets are still supported.
- Some system classes cannot be overridden (for example, you cannot implement a subclass).
- Some platform features that require provisioning can't work in the Xamarin Live Player app (however it has been configured for common operations like photo gallery access).
- Custom targets and build steps are ignored. For example, tools like Fody cannot be incorporated.

> [!WARNING]
> **NOTE:** Xamarin Live Player does not work with Xamarin Studio

Please report any additional issues on [bugzilla](https://aka.ms/live-player-report-issue).


## Related Links

- [Troubleshooting](~/tools/live-player/troubleshooting.md)
- [Setup](~/tools/live-player/install.md)
