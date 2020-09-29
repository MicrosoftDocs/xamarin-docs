---
title: "File access with Xamarin.Android"
description: "This guide will explain file access in Xamarin.Android"
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
---

# File Storage and Access with Xamarin.Android

A common requirement for Android apps is to manipulate files &ndash; saving pictures, downloading documents, or exporting data to share with other programs. Android (which is based on Linux) supports this by providing space for file storage. Android groups the filesystem into two different types of storage:

* **Internal Storage** &ndash; this is a portion of the file system that can be accessed only by the application or the operating system.
* **External Storage** &ndash; this is a partition for the storage of files that is accessible by all apps, the user, and possibly other devices. On some devices, external storage may be removable (such as an SD card).

These groupings are conceptual only, and don't necessarily refer to a single partition or directory on the device. An Android device will always provide partition for internal storage and external storage. It is possible that certain devices may have multiple partitions that are considered to be external storage. Regardless of the partition the APIs for reading, writing, or creating files is the same. There are two sets of APIs that a Xamarin.Android application may use for file access:

1. **The .NET APIs (provided by Mono and wrapped by Xamarin.Android)** &ndash;  These includes the [file system helpers](~/essentials/file-system-helpers.md?context=xamarin/android) provided by [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). The .NET APIs provide the best cross-platform compatibility and as such the focus of this guide will be on these APIs.
1. **The native Java file access APIs (provided by Java and wrapped by Xamarin.Android)** &ndash; Java provides its own APIs for reading and writing files. These are a completely acceptable alternative to the .NET APIs, but are specific to Android and are not suitable for apps that are intended to be cross-platform.

Reading and writing to files is almost identical in Xamarin.Android as it is to any other .NET application. The Xamarin.Android app determines the path to the file that will be manipulated, then uses standard .NET idioms for file access. Because the actual paths to internal and external storage may vary from device to device or from Android version to Android version, it is not recommended to hard code the path to the files. Instead, use the Xamarin.Android APIs to determine the path to files. That way, the .NET APIs for reading and writing files exposes the native Android APIs that will help with determining the path to files on internal and external storage.

Before discussing the APIs involved with file access, it is important to understand some of the details surrounding internal and external storage. This will be discussed in the next section.

## Internal vs external storage

Conceptually, internal storage and external storage are very similar &ndash; they are both places at which a Xamarin.Android app may save files. This similarity may be confusing for developers who are not familiar with Android as it is not clear when an app should use internal storage vs external storage.

Internal storage refers to the non-volatile memory that Android allocates to the operating system, APKs, and for individual apps. This space is not accessible except by the operating system or apps. Android will allocate a directory in the internal storage partition for each app. When the app is uninstalled, all the files that are kept on internal storage in that directory will also be deleted. Internal storage is best suited for files that are only accessible to the app and that will not be shared with other apps or will have very little value once the app is uninstalled. On Android 6.0 or higher, files on internal storage may be automatically backed up by Google using the [Auto Backup feature in Android 6.0](https://developer.android.com/guide/topics/data/autobackup). Internal storage has the following disadvantages:

* Files cannot be shared.
* Files  will be deleted when the app is uninstalled.
* The space available on internal storage maybe limited.

External storage refers to file storage that is not internal storage and not exclusively accessible to an app. The primary purpose of external storage is to provide a place to put files that are meant to be shared between apps or that are too large to fit on the internal storage. The advantage of external storage is that it typically has much more space for files than internal storage. However, external storage is not always guaranteed to be present on a device and may require special permission from the user to access it.

> [!NOTE]
> For devices that support multiple users, Android will provide each user their own directory on both internal and external storage. This directory is inaccessible to other users on the device. This separation is invisible to apps as long as they do not hardcode paths to files on internal or external storage.

As a rule of thumb, Xamarin.Android apps should prefer saving their files on internal storage when it is reasonable, and rely on external storage when files need to be shared with other apps, are very large, or should be retained even if the app is uninstalled. For example, a configuration file is best suited for a internal storage as it has no importance except to the app that creates it.  In contrast, photos are a good candidate for external storage. They can be very large and in many cases the user may want to share them or access them even if the app is uninstalled.

This guide will focus on internal storage. Please see the guide [External storage](~/android/platform/files/external-storage.md) for details on using external storage in a Xamarin.Android application.

## Working with internal storage

The internal storage directory for an application is determined by the operating system, and is exposed to Android apps by the `Android.Content.Context.FilesDir` property. This will return a `Java.IO.File` object representing the directory that Android has dedicated exclusively for the app.  For example, an app with the package name **com.companyname** the internal storage directory might be:

```bash
/data/user/0/com.companyname/files
```

This document will refer to the internal storage directory as _INTERNAL\_STORAGE_.

> [!IMPORTANT]
> The exact path to the internal storage directory can vary from device to device and between versions of Android. Because of this, apps must not hard code the path to the internal files storage directory, and instead use the Xamarin.Android APIs, such as `System.Environment.GetFolderPath()`.

To maximize code sharing, Xamarin.Android apps (or Xamarin.Forms apps targeting Xamarin.Android) should use the [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*) method. In Xamarin.Android, this method will return a string for a directory that is the same location as `Android.Content.Context.FilesDir`. This method takes an enum, `System.Environment.SpecialFolder`, which is used to identify a set of enumerated constants that represent the paths of special folders used by the operating system. Not all of the `System.Environment.SpecialFolder` values will map to a valid directory on Xamarin.Android. The following table describes what path can be expected for a given value of `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Path  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNAL\_STORAGE_/Desktop** |
| `LocalApplicationData` | **_INTERNAL\_STORAGE_/.local/share** |
| `MyDocuments` | **_INTERNAL\_STORAGE_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_INTERNAL\_STORAGE_/Pictures** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_INTERNAL\_STORAGE_** |
| `Fonts` | **_INTERNAL\_STORAGE_/.fonts** |
| `Templates` | **_INTERNAL\_STORAGE_/Templates** |
| `CommonApplicationData` | **/usr/share** |
| `CommonApplicationData` | **/usr/share** |

### Reading or Writing to files on internal storage

Any of the [C# APIs for writing](/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) to a file are sufficient; all that is necessary is to get the path to the file that is in the directory allocated to the application. It is strongly recommended that the async versions of the .NET APIs are used to minimize any issues that may be associate with file access blocking the main thread.

This code snippet is one example of writing an integer to a UTF-8 text file to the internal storage directory of an application:

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

The next code snippet provides one way to read an integer value that was stored in a text file:

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## Using  Xamarin.Essentials &ndash; File System Helpers

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) is a set of APIs for writing cross-platform compatible code. The [File System Helpers](~/essentials/file-system-helpers.md?context=xamarin/android) is a class that contains a series of helpers to simplify locating the application's cache and data directories. This code snippet provides an example of how to find the internal storage directory and the cache directory for an app:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## Hiding files from the `MediaStore`

The `MediaStore` is an Android component that collects meta data about media files (videos, music, images) on an Android device. Its purpose is simplify the sharing of these files across all Android apps on the device.

Private files will not show up as shareable media. For example, if an app saves a picture to its private external storage, then that file will not be picked up by the media scanner (`MediaStore`).

Public files will be picked up by `MediaStore`. Directories that have a zero byte file name **.NOMEDIA** will not be scanned by `MediaStore`.

## Related Links

* [External Storage](~/android/platform/files/external-storage.md)
* [Save files on device storage](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials File System Helpers](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Backup user data with Auto Backup](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable Storage](https://source.android.com/devices/storage/adoptable)