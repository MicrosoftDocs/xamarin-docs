---
title: "Xamarin.Essentials Launcher"
description: "The Launcher class in Xamarin.Essentials enables an application to open a URI by the system."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
---

# Xamarin.Essentials: Launcher

The **Launcher** class enables an application to open a URI by the system. This is often used when deep linking into another application's custom URI schemes. If you are looking to open the browser to a website then you should refer to the **[Browser](open-browser.md)** API.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Launcher

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To use the Launcher functionality call the `OpenAsync` method and pass in a `string` or `Uri` to open. Optionally, the `CanOpenAsync` method can be used to check if the URI schema can be handled by an application on the device.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## Platform Differences

# [Android](#tab/android)

The Task returned from `CanOpenAsync` completes immediately.

# [iOS](#tab/ios)

If the destination application on this device has never been opened by `OpenAsync` from your application before, iOS will prompt the user once to allow your app to open it.

The Task returned from `CanOpenAsync` completes immediately.

More information about the iOS implementation is available [here](https://developer.xamarin.com/api/member/UIKit.UIApplication.CanOpenUrl/p/Foundation.NSUrl/)

# [UWP](#tab/uwp)

No platform differences.

-----

## API

- [Launcher source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API documentation](xref:Xamarin.Essentials.Launcher)
