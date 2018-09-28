---
title: "Xamarin.Essentials: Power Energy Saver Status"
description: "The Power class allows a program to obtain the energy-saver status to determine if the device is operating in a low-power mode."
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: jamesmontemagno
ms.author: jamont
ms.date: 06/27/2018
---

# Xamarin.Essentials: Power Energy Saver Status

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Power** class provides information about the device's energy-saver status, which indicates if the device is running in a low-power mode. Applications should avoid background processing if the device's energy-saver status is on.

## Background

Devices that run on batteries can be put into a low-power energy-saver mode. Sometimes devices are switched into this mode automatically, for example, when the battery drops below 20% capacity. The operating system responds to energy-saver mode by reducing activities that tend to deplete the battery. Applications can help by avoiding background processing or other high-power activities when energy-saver mode is on.

For Android devices, the **Power** class returns meaningful information only for Android version 5.0 (Lollipop) and above.

## Using the Power class

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

Obtain the current energy-saver status of the device using the static `Power.EnergySaverStatus` property:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

This property returns a member of the `EnergySaverStatus` enumeration, which is either `On`, `Off`, or `Unknown`. If the property returns `On`, the application should avoid background processing or other activities that might consume a lot of power.

The application should also install an event handler. The **Power** class exposes an event that is triggered when the energy-saver status changes:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

If the energy-saver status changes to `On`, the application should stop performing background processing. If the status changes to `Unknown` or `Off`, the application can resume background processing.

## API

- [Power source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power API documentation](xref:Xamarin.Essentials.Power)
