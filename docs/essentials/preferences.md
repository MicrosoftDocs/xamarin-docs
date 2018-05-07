---
title: "Xamarin.Essentials Preferences"
description: "The Preferences class saves application preferences in a key/value store."
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Preferences

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Preferences** class helps to store application preferences in a key/value store.

## Using Secure Storage

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

## Platform Implementation Specifics

# [Android](#tab/android)

All data is stored into [Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html). If no `sharedName` is specified the default shared preferences are used, else the name is used to get a **private** shared preferences with the specified name.

# [iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) is used to store values on iOS devices. If no `sharedName` is specified the `StandardUserDefaults` are used, else the name is used to create a new `NSUserDefaults` with the specified name used for the `NSUserDefaultsType.SuiteName`.

# [UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) is used to store the values on the device. If no `sharedName` is specified the `LocalSettings` are used, else the name is used to create a new container inside of `LocalSettings`.

--------------

## Limitations

When storing a string, this API is intended to store small amounts of text.  Performance may be subpar if you try to use it to store large amounts of text.

## API

- [Preferences source code](https://github.com/xamarin/Essentials/tree/master/Essentials/Preferences)
- [Preferences API documentation](xref:Xamarin.Essentials.Preferences)
