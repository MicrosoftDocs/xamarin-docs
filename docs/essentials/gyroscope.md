---
title: "Xamarin.Essentials: Gyroscope"
description: "The Gyroscope class in Xamarin.Essentials lets you monitor the device's gyroscope sensor, which measures rotation around the device's three primary axes."
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: Gyroscope

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Gyroscope** class lets you monitor the device's gyroscope sensor which is the rotation around the device's three primary axes.

## Using Gyroscope

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Gyroscope functionality works by calling the `Start` and `Stop` methods to listen for changes to the gyroscope. Any changes are sent back through the `ReadingChanged` event. Here is sample usage:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## API

- [Gyroscope source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroscope API documentation](xref:Xamarin.Essentials.Gyroscope)
