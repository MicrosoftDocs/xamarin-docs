---
title: "Windows Platform Features"
description: "This article explains the Windows platform support that's available in Xamarin.Forms."
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
---

# Windows Platform Features

Developing Xamarin.Forms applications for Windows platforms
requires Visual Studio. The [requirements page](~/xamarin-forms/get-started/installation.md)
contains more information about the pre-requisites.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## Platform Support

The Xamarin.Forms templates available in Visual Studio contain a Universal Windows Platform (UWP) project.

> [!NOTE]
> Xamarin.Forms 1.x and 2.x support _Windows Phone 8 Silverlight_,
> _Windows Phone 8.1_, and _Windows 8.1_ application development. However, these project types have been deprecated.

## Getting Started

Go to **File > New > Project** in Visual Studio and choose one of the
**Cross-Platform > Blank App (Xamarin.Forms)** templates to get
started.

Older Xamarin.Forms solutions, or those created on macOS, will not have
all the Windows projects listed above (but they need to be manually added).
If the Windows platform you wish to target isn't already in your solution,
vist the [setup instructions](installation/index.md) to add the desired
Windows project type/s.

## Samples

[All the samples](https://github.com/xamarin/xamarin-forms-book-preview-2)
for Charles Petzold's book
[*Creating Mobile Apps with Xamarin.Forms*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
include Universal Windows Platform (for Windows 10) projects.

The ["Scott Hanselman" demo app](https://github.com/jamesmontemagno/Hanselman.Forms)
is available separately, and also includes Apple Watch and Android Wear projects
(using Xamarin.iOS and Xamarin.Android respectively, Xamarin.Forms does not run
on those platforms).

## Related Links

- [Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md)
