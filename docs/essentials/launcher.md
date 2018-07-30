---
title: "Xamarin.Essentials Launcher"
description: "The Launcher class in Xamarin.Essentials enables an application to open a URI by the system."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
---

# Xamarin.Essentials: Launcher

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Launcher** class enables an application to open a URI by the system. This is often used when deep linking into another application's custom URI schemes. If you are looking to open the browser to a website then you should refer to the **[Browser](open-browser.md)** API.

## Using Launcher

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To use the Launcher functionality call the `OpenAsync` method and pass in a `string` or `Uri` to open. Optionally, the `CanOpenAsync` method can be used to check if the URI schema can be handled by an application on the device.

```csharp
public class LauncherTest
{
    public async Task OpenRideShare()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## API

- [Launcher source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API documentation](xref:Xamarin.Essentials.Launcher)
