---
title: "Xamarin.Essentials: Compass"
description: "This document describes the Compass class in Xamarin.Essentials, which lets you monitor the device's magnetic north heading."
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: Compass

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Compass** class lets you monitor the device's magnetic north heading.

## Using Compass

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Compass functionality works by calling the `Start` and `Stop` methods to listen for changes to the compass. Any changes are sent back through the `ReadingChanged` event. Here is an example:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## Platform Implementation Specifics

# [Android](#tab/android)

Android does not provide a API for retrieving the compass heading. We utilize the accelerometer and magnetometer to calculate the magnetic north heading, which is recommended by Google. 

In rare instances, you maybe see inconsistent results because the sensors need to be calibrated, which involves moving your device in a figure-8 motion. The best way of doing this is to open Google Maps, tap on the dot for your location, and select **Calibrate compass**.

Be aware that running multiple sensors from your app at the same time may adjust the sensor speed.

--------------

## API

- [Compass source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass API documentation](xref:Xamarin.Essentials.Compass)
