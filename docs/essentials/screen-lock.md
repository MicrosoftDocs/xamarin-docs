---
title: "Xamarin.Essentials Screen Lock"
description: "The ScreenLock class can request to keep the screen from falling asleep when the application is running."
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Screen Lock

![Pre-release NuGet](~/media/shared/pre-release.png)

The **ScreenLock** class can request to keep the screen from falling asleep when the application is running.

## Using ScreenLock

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The screen lock functionality works by calling the `RequestActive` and `RequestRelease` methods to request the screen from turning off.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## API

- [Screen Lock source code](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Screen Lock API documentation](xref:Xamarin.Essentials.ScreenLock)
