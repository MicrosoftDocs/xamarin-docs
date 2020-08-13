---
title: "Xamarin.Essentials Open Browser"
description: "The Browser class in Xamarin.Essentials enables an application to open a web link in the optimized system preferred browser or the external browser."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
no-loc: [Xamarin.Forms, Xamarin.Essentials]
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
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

This method returns after the browser was _launched_ and not necessarily _closed_ by the user.  The `bool` result indicates whether the launching was successful or not.

## Customization

When using the system preferred browser there are several customization options available for iOS and Android. This includes a `TitleMode` (Android only), and preferred color options for the `Toolbar` (iOS and Android) and `Controls` (iOS only) that appear.

These options are specified using `BrowserLaunchOptions` when calling `OpenAsync`.

```csharp
await Browser.OpenAsync(uri, new BrowserLaunchOptions
                {
                    LaunchMode = BrowserLaunchMode.SystemPreferred,
                    TitleMode = BrowserTitleMode.Show,
                    PreferredToolbarColor = Color.AliceBlue,
                    PreferredControlColor = Color.Violet
                });
```

![Browser Options](images/browser-options.png)

## Platform Implementation Specifics

# [Android](#tab/android)

The Launch Mode determines how the browser is launched:

## System Preferred

[Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs) will attempted to be used to load the Uri and keep navigation awareness.

## External

An `Intent` will be used to request the Uri be opened through the systems normal browser.

# [iOS](#tab/ios)

## System Preferred

[SFSafariViewController](xref:SafariServices.SFSafariViewController) is used to load the Uri and keep navigation awareness.

## External

The standard `OpenUrl` on the main application is used to launch the default browser outside of the application.

# [UWP](#tab/uwp)

The user's default browser will always be launched regardless of the `BrowserLaunchMode`.

--------------

## API

- [Browser source code](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Browser)
- [Browser API documentation](xref:Xamarin.Essentials.Browser)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Open-Browser-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
