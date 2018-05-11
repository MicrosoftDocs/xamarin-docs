---
title: "Xamarin.Essentials Device Information"
description: "The DeviceInfo class provides information about the device the application is running on."
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Device Information

![Pre-release NuGet](~/media/shared/pre-release.png)

The **DeviceInfo** class provides information about the device the application is running on.

## Using DeviceInfo

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The following information is exposed through the API:

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## [Platforms](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` correlates to a constant string that maps to the operating system. The values can be checked with the `Platforms` class:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – Unsupported

## [Idioms](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` correlates a constant string that maps to the type of device the application is running on. The values can be checked with the `Idioms` class:

- **DeviceInfo.Idioms.Phone** – Phone
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – Desktop
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – Unsupported

## Device Type

`DeviceInfo.DeviceType` correlates an enumeration to determine if the application is runing on a physical or vitual device. A virtual device is a simulator or emulator.

## API

- [DeviceInfo source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API documentation](xref:Xamarin.Essentials.DeviceInfo)
