---
title: "Xamarin.Essentials Open Browser"
description: "The Browser class in Xamarin.Essentials enables an application to open a web link in the optimized system preferred browser or the external browser."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
---

# Xamarin.Essentials: Browser

The **Browser** class enables an application to open a web link in the optimized system preferred browser or the external browser.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Browser

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Browser functionality works by calling the `OpenAsync` method with the `Uri` and `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task<bool> OpenBrowser(Uri uri)
    {
        return await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

This method returns after the browser was _launched_ and not necessarily _closed_ by the user.  The `bool` result indicates whether the launching was successful or not.

## Platform Implementation Specifics

# [Android](#tab/android)

The Launch Mode determines how the browser is launched:

## System Preferred

[Chrome Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs) will attempted to be used load the Uri and keep navigation awareness.

## External

An `Intent` will be used to request the Uri be opened through the systems normal browser.

# [iOS](#tab/ios)

## System Preferred

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) is used to load the Uri and keep navigation awareness.

## External

The standard `OpenUrl` on the main application is used to launch the default browser outside of the application.

# [UWP](#tab/uwp)

The user's default browser will always be launched regardless of the `BrowserLaunchMode`.

--------------

## API

- [Browser source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Browser API documentation](xref:Xamarin.Essentials.Browser)
