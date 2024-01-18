---
title: "Default image directory on Windows"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the Windows platform-specific that defines the directory in the project that image assets will be loaded from."
ms.service: xamarin
ms.assetid: 537A032B-74DD-4D43-864E-7D7113286D0D
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Default image directory on Windows

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This Universal Windows Platform platform-specific defines the directory in the project that image assets will be loaded from. It's consumed in XAML by setting the `Application.ImageDirectory` to a `string` that represents the project directory that contains image assets:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
	...
</Application>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

The `Application.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The `Application.SetImageDirectory` method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to specify the project directory that images will be loaded from. In addition, the `GetImageDirectory` method can be used to return a `string` that represents the project directory that contains the application image assets.

The result is that all images used in an application will be loaded from the specified project directory.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)