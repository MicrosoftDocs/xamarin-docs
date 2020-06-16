---
title: "Xamarin.Essentials: Share"
description: "The Share class in Xamarin.Essentials enables an application to share data such as text and web links to other applications on the device."
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
no-loc: [Xamarin.Forms, Xamarin.Essentials]
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

![Share](images/share.png)

## Files

This features enables an app to share files to other applications on the device. Xamarin.Essentials will automatically detect the file type (MIME) and request a share. Each platform may only support specific file extensions.

Here is a sample of writing text to disk and sharing it to other apps:

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## Presentation Location

When requesting a share on iPadOS you have the ability to present in a pop over control. You can specify the location using the `PresentationSourceBounds` property:

```csharp
await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file),
    PresentationSourceBounds = DeviceInfo.Platform== DevicePlatform.iOS && DeviceInfo.Idiom == DeviceIdiom.Tablet
                            ? new System.Drawing.Rectangle(0, 20, 0, 0)
                            : System.Drawing.Rectangle.Empty
});
```

## Platform Differences

# [Android](#tab/android)

- `Subject` property is used for desired subject of a message.

# [iOS](#tab/ios)

- `Subject` not used.
- `Title` not used.

# [UWP](#tab/uwp)

- `Title` will default to Application Name if not set.
- `Subject` not used.

-----

## API

- [Share source code](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Share)
- [Share API documentation](xref:Xamarin.Essentials.Share)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
