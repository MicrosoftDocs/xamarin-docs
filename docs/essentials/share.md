---
title: "Xamarin.Essentials: Share"
description: "The Share class in Xamarin.Essentials enables an application to share data such as text and web links to other applications on the device."
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
---

# Xamarin.Essentials: Share

The **Share** class enables an application to share data such as text and web links to other applications on the device.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Share

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Share functionality works by calling the `RequestAsync` method with a data request payload that includes information to share to other applications. Text and Uri can be mixed and each platform will handle filtering based on content.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

User interface to share to external application that appears when request is made:

![Share](share-images/share.png)

## Platform Differences

# [Android](#tab/android)

* `Subject` property is used for desired subject of a message.

# [iOS](#tab/ios)

* `Subject` not used.
* `Title` not used.

# [UWP](#tab/uwp)

* `Title` will default to Application Name if not set.
* `Subject` not used.

-----

## API

- [Share source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share API documentation](xref:Xamarin.Essentials.Share)
