---
title: "Xamarin.Essentials: Device Display Information"
description: "This document describes the DeviceDisplay class in Xamarin.Essentials, which provides screen metrics for the device on which the application is running."
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: Device Display Information

![Pre-release NuGet](~/media/shared/pre-release.png)

The **DeviceDisplay** class provides information about the device's screen metrics the application is running on.

## Using DeviceDisplay

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

## Screen Metrics

In addition to basic device information the **DeviceDisplay** class contains information about the device's screen and orientation.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

The **DeviceDisplay** class also exposes an event that can be subscribed to that is triggered whenever any screen metric changes:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## API

- [DeviceDisplay source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API documentation](xref:Xamarin.Essentials.DeviceDisplay)
