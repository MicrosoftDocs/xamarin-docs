---
title: "Xamarin.Essentials: File Picker"
description: "The FilePicker class in Xamarin.Essentials lets a user pick a single or multiple files from the device."
ms.assetid: 00bdbd57-56b1-47ca-8abe-cebe1b01f61a
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Essentials: File Picker

The **FilePicker** class lets a user pick a single or multiple files from the device.

## Get started

[!include[](~/essentials/includes/get-started.md)]

To access the **FilePicker** functionality the following platform specific setup is required.

# [Android](#tab/android)

The `ReadExternalStorage` permission is required and must be configured in the Android project. This can be added in the following ways:

Open the **AssemblyInfo.cs** file under the **Properties** folder and add:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadExternalStorage)]
```

OR Update Android Manifest:

Open the **AndroidManifest.xml** file under the **Properties** folder and add the following inside of the **manifest** node.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Or right click on the Android project and open the project's properties. Under **Android Manifest** find the **Required permissions:** area and check this permission. This will automatically update the **AndroidManifest.xml** file.

# [iOS](#tab/ios)

No additional setup required.

# [UWP](#tab/uwp)

No additional setup required.

-----

> [!TIP]
> All methods must be called on the UI thread because permission checks and requests are automatically handled by Xamarin.Essentials.

## Pick File

`FilePicker.PickAsync()` method enables your user to pick a file from the device. You are able to specific different `PickOptions` when calling the method enabling you to specify the title to display and the file types the user is allowed to pick. By default

```csharp
async Task<FileResult> PickAndShow(PickOptions options)
{
    try
    {
        var result = await FilePicker.PickAsync();
        if (result != null)
        {
            Text = $"File Name: {result.FileName}";
            if (result.FileName.EndsWith("jpg", StringComparison.OrdinalIgnoreCase) ||
                result.FileName.EndsWith("png", StringComparison.OrdinalIgnoreCase))
            {
                var stream = await result.OpenReadAsync();
                Image = ImageSource.FromStream(() => stream);
            }
        }
    }
    catch (Exception ex)
    {
        // The user canceled or something went wrong
    }
}
```

Default file types are provided with `FilePickerFileType.Images`, `FilePickerFileType.Png`, and `FilePickerFilerType.Videos`. You can specify custom files types when creating the `PickOptions` and they can be customized per platform. For example here is how you would specify specific comic file types:

```csharp
var customFileType =
    new FilePickerFileType(new Dictionary<DevicePlatform, IEnumerable<string>>
    {
        { DevicePlatform.iOS, new[] { "public.my.comic.extension" } }, // or general UTType values
        { DevicePlatform.Android, new[] { "application/comics" } },
        { DevicePlatform.UWP, new[] { ".cbr", ".cbz" } },
        { DevicePlatform.Tizen, new[] { "*/*" } },
        { DevicePlatform.macOS, new[] { "cbr", "cbz" } }, // or general UTType values
    });
var options = new PickOptions
{
    PickerTitle = "Please select a comic file",
    FileTypes = customFileType,
};
```

## Pick Multiple Files

If you desire your user to pick multiple files you can call the `FilePicker.PickMultipleAsync()` method. It also takes in `PickOptions` as a parameter to specify additional information. The results are the same as `PickAsync`, but instead of a single `FileResult` an `IEnumerable<FileResult>` is returned that can be iterated over.


## Platform Differences

# [Android](#tab/android)

- The URI of the resulting file may not be persisted between reboots.

# [iOS](#tab/ios)

No platform differences.

# [UWP](#tab/uwp)

No platform differences.

-----

## API

- [FilePicker source code](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/FilePicker)
- [FilePicker API documentation](xref:Xamarin.Essentials.FilePicker)
