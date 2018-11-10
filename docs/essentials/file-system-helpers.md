---
title: "Xamarin.Essentials: File System Helpers"
description: "The FileSystem class in Xamarin.Essentials contains a series of helpers to find the application's cache and data directories and open files inside of the app package."
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: File System Helpers

![Pre-release NuGet](~/media/shared/pre-release.png)

The **FileSystem** class contains a series of helpers to find the application's cache and data directories and open files inside of the app package.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using File System Helpers

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To get the application's directory to store **cache data**. Cache data can be used for any data that needs to persist longer than temporary data, but should not be data that is required to properly operate.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

To get the application's top-level directory for any files that are not user data files. These files are backed up with the operating system syncing framework. See Platform Implementation Specifics below.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

To open a file that is bundled into the application package:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## Platform Implementation Specifics

# [Android](#tab/android)

- **CacheDirectory** – Returns the [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) of the current context.
- **AppDataDirectory** – Returns the [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) of the current context and are backed up using [Auto Backup](https://developer.android.com/guide/topics/data/autobackup.html) starting on API 23 and above.

Add any file into the **Assets** folder in the Android project and mark the Build Action as **AndroidAsset** to use it with `OpenAppPackageFileAsync`.

# [iOS](#tab/ios)

- **CacheDirectory** – Returns the [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directory.
- **AppDataDirectory** – Returns the [Library](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directory that is backed up by iTunes and iCloud.

Add any file into the **Resources** folder in the iOS project and mark the Build Action as **BundledResource** to use it with `OpenAppPackageFileAsync`.

# [UWP](#tab/uwp)

- **CacheDirectory** – Returns the [LocalCacheFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) directory..
- **AppDataDirectory** – Returns the [LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) directory that is backed up to the cloud.

Add any file into the root in the UWP project and mark the Build Action as **Content** to use it with `OpenAppPackageFileAsync`.

--------------

## API

- [File System Helpers source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [File System API documentation](xref:Xamarin.Essentials.FileSystem)
