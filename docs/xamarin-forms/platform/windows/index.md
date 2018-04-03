---
title: "Windows Platform Features"
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

The Xamarin.Forms templates available in Visual Studio contain one
Windows project by default:

* **Universal Windows Platform Apps** - Xamarin.Forms apps can also be
  optimized for Windows 10. Universal (UWP) apps can run on phone, tablet, and
  desktop devices.

If you have installed the correct development options in Visual Studio,
  it's also possible to [add](installation/index.md) these project types to
  support older versions of Windows:

* **Windows 8.1** - You can deploy Xamarin.Forms apps to tablet and desktop
  form-factors as a Windows 8.1 app project using WinRT controls.
* **Windows Phone 8.1** - Xamarin.Forms has full support
  for the Windows Phone 8.1 platform using WinRT. The look and feel of apps using
  Windows Phone 8.1 support may be different to your earlier Xamarin.Forms Windows Phone
  apps that were based on Silverlight.


> [!NOTE]
> Xamarin.Forms 1.x and 2.x support _Windows Phone 8 Silverlight_
> application development, however this project type has been deprecated.


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
include Windows Phone 8.1, Windows 8.1, and Universal Windows Platform (for Windows 10)
projects.

The ["Scott Hanselman" demo app](https://github.com/jamesmontemagno/Hanselman.Forms)
is available separately, and also includes Apple Watch and Android Wear projects
(using Xamarin.iOS and Xamarin.Android respectively, Xamarin.Forms does not run
on those platforms).


## Related Links

- [Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md)
