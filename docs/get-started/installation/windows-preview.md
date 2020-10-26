---
title: "Installing Xamarin Preview on Windows"
description: "This document describes how to install a preview version of Xamarin on Visual Studio 2019 by using the Preview release channel."
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Installing Xamarin Preview on Windows

Visual Studio 2019 and Visual Studio 2017 do not support alpha, beta, and stable channels in
the same way as earlier versions. Instead, there are just two options:

- **Release** – equivalent to the _Stable_ channel in Visual Studio for Mac
- **Preview** – equivalent to the _Alpha_ and _Beta_ channels in Visual Studio for Mac

> [!TIP]
> To try out pre-release features, you should [download the Visual Studio Preview installer](https://visualstudio.microsoft.com/vs/preview/), which will offer the option to install **Preview**
> versions of Visual Studio side-by-Side with the stable (Release) version. More information on What's new in Visual Studio 2019 can be found in the [release notes](/visualstudio/releases/2019/release-notes).

The Preview version of Visual Studio may include corresponding Preview
versions of Xamarin functionality, including:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Inspector
- Xamarin Remote iOS Simulator

The **Preview Installer** screenshot below shows both Preview and Release options (notice the grey version numbers: version 15.0 is release and version 15.1 is a Preview):

![installer showing preview options](windows-images/vs2017-installer.jpg)

During the installation process, an **Installation Nickname** can be
applied to the side-by-side installation (so they can be distinguished
in the Start menu), as shown below:

[![edit nickname before installing](windows-images/vs2017-nickname-sml.png "edit nickname before installing")](windows-images/vs2017-nickname.png#lightbox)

### Uninstalling Visual Studio 2019 Preview

The **Visual Studio Installer** should also be used to un-install preview versions of Visual Studio 2019. Read the [uninstalling Xamarin guide](uninstalling-xamarin.md#uninstallvs2017) for more information.