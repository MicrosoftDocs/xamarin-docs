---
title: "Xamarin.Essentials: Preferences"
description: "This document describes the Preferences class in Xamarin.Essentials, which saves application preferences in a key/value store. It discusses how to use the class and the types of data that can be stored."
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
---

# Xamarin.Essentials: Preferences

The **Preferences** class helps to store application preferences in a key/value store.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Preferences

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To save a value for a given _key_ in preferences:

```csharp
Preferences.Set("my_key", "my_value");
```

To retrieve a value from preferences or a default if not set:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

To remove the _key_ from preferences:

```csharp
Preferences.Remove("my_key");
```

To remove all preferences:

```csharp
Preferences.Clear();
```

In addition to these methods each take in an optional `sharedName` that can be used to create additional containers for preference. Read the platform implementation specifics below.

## Supported Data Types

The following data types are supported in **Preferences**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## Implementation Details

Values of `DateTime` are stored in a 64-bit binary (long integer) format using two methods defined by the `DateTime` class: The [`ToBinary`](xref:System.DateTime.ToBinary) method is used to encode the `DateTime` value, and the [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) method decodes the value. See the documentation of these methods for adjustments that might be made to decoded values when a `DateTime` is stored that is not a Coordinated Universal Time (UTC) value.

## Platform Implementation Specifics

# [Android](#tab/android)

All data is stored into [Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html). If no `sharedName` is specified the default shared preferences are used, else the name is used to get a **private** shared preferences with the specified name.

# [iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults) is used to store values on iOS devices. If no `sharedName` is specified the `StandardUserDefaults` are used, else the name is used to create a new `NSUserDefaults` with the specified name used for the `NSUserDefaultsType.SuiteName`.

# [UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) is used to store the values on the device. If no `sharedName` is specified the `LocalSettings` are used, else the name is used to create a new container inside of `LocalSettings`.

--------------

## Persistence

Uninstalling the application will cause all _Preferences_ to be removed. There is one exception to this, which for apps that target and run on Android 6.0 (API level 23) or later that are using [__Auto Backup__](https://developer.android.com/guide/topics/data/autobackup). This feature is on by default and preserves app data including __Shared Preferences__, which is what the **Preferences** API utilizes. You can disable this by following Google's [documentation](https://developer.android.com/guide/topics/data/autobackup).

## Limitations

When storing a string, this API is intended to store small amounts of text.  Performance may be subpar if you try to use it to store large amounts of text.

## API

- [Preferences source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Preferences API documentation](xref:Xamarin.Essentials.Preferences)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
