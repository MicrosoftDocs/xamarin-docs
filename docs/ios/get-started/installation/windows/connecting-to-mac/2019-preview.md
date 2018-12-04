---
title: "Connecting Visual Studio 2019 Previews on Windows and macOS"
description: "This guide provides instructions on how to use the Visual Studio 2019 preview on Windows to build iOS apps, while using the Visual Studio 2019 for Mac preview on macOS to host your builds."
ms.prod: xamarin
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/04/2018
---
# Visual Studio 2019 preview pairing

![Preview](~/media/shared/preview.png)

The [Visual Studio for Mac 2019 preview](https://docs.microsoft.com/visualstudio/mac/install-preview) is the first time that Visual Studio side-by-side installation has been fully supported on macOS. This creates a new situation for Windows developers building iOS applications: if their Mac Build Host has both Visual Studio 2017 for Mac (version 7.x) and the preview of Visual Studio 2019 for Mac (version 8.0), which one is performing as the build host for their Windows installation?

_By default_, the stable release &ndash; Visual Studio 2017 for Mac (version 7.7) &ndash; will be used by Visual Studio on Windows, regardless of whether you are using Visual Studio 2017 or Visual Studio 2019 preview on Windows.

To properly test the Visual Studio 2019 previews on both Windows and macOS:

1. Install and use the Visual Studio 2019 (version 16.0) preview on your Windows computer.
2. Install the Visual Studio 2019 for Mac (version 8.0) preview on your Mac.
3. In **Visual Studio 2019 for Mac**:

    a. Choose the **Visual Studio > Check for updates** menu item.

    b. In the **Visual Studio Update** window, select **Xamarin Preview** from the update channel. The following warning will appear:

    > [!WARNING]
    > Try the latest Visual Studio 2019 for Mac Preview builds with the latest Mono runtime and Xamarin SDKs. **Installing builds from this channel will break the Visual Studio 2017 for Mac release.** Use this channel only if you are building Xamarin apps on a Windows operating system using the Visual Studio 2019 Preview releases.

    c. Press the **Switch channel** button to enable the preview build host.

4. Follow the [Pair to Mac for Xamarin.iOS development](index.md) instructions, and [troubleshooting tips](troubleshooting.md) (if required).