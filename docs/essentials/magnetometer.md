---
title: "Xamarin.Essentials Magnetometer"
description: "The Magnetometer class lets you monitor the device's magnetometer sensor which indicates the device's orientation relative to Earth's magnetic field."
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Magnetometer

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Magnetometer** class lets you monitor the device's magnetometer sensor which indicates the device's orientation relative to Earth's magnetic field.

## Using Magnetometer

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Magnetometer functionality works by calling the `Start` and `Stop` methods to listen for changes to the magnetometer. Any changes are sent back through the `ReadingChanged` event. Here is sample usage:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

All data is returned in microteslas.

## [Sensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Fastest** – Get the sensor data as fast as possible (not guaranteed to return on UI thread).
- **Game** – Rate suitable for games (not guaranteed to return on UI thread).
- **Normal** – Default rate suitable for screen orientation changes.
- **Ui** – Rate suitable for general user interface.

## API

- [Magnetometer source code](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [Magnetometer API documentation](xref:Xamarin.Essentials.Magnetometer)
