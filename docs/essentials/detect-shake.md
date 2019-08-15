---
title: "Xamarin.Essentials: Detect Shake"
description: "The Accelerometer class in Xamarin.Essentials enables you to detect a shake movement of the device."
ms.assetid: 07513D32-120F-4F12-8757-A47802A8027B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/28/2019
ms.custom: video
---

# Xamarin.Essentials: Detect Shake

The **[Accelerometer](accelerometer.md)** class lets you monitor the device's accelerometer sensor, which indicates the acceleration of the device in three-dimensional space. Additionally, it enables you to register for events when the user shakes the device.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Detect Shake

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To detect a shake of the device you must use the Accelerometer functionality by calling the `Start` and `Stop` methods to listen for changes to the acceleration and to detect a shake. Any time a shake is detected a `ShakeDetected` event will fire. It is recommended to use `Game` or faster for the `SensorSpeed`. Here is sample usage:

```csharp

public class DetectShakeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Game;

    public DetectShakeTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ShakeDetected  += Accelerometer_ShakeDetected ;
    }

    void Accelerometer_ShakeDetected (object sender, EventArgs e)
    {
        // Process shake event
    }

    public void ToggleAccelerometer()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

## Implementation Details

The detect shake API uses raw readings from the accelerometer to calculate acceleration. It uses a simple queue mechanism to detect if 3/4ths of the recent accelerometer events occurred in the last half second. Acceleration is calculated by adding the square of the X, Y, and Z readings from the accelerometer and comparing it to a specific threashold.

## API

- [Accelerometer source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Accelerometer API documentation](xref:Xamarin.Essentials.Accelerometer)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Detect-Shake-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
