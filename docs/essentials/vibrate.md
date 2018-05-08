---
title: "Xamarin.Essentials Vibration"
description: "The Vibration class lets you start and stop the vibrate functionality for a desired amount of time."
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Vibration

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Vibration** class lets you start and stop the vibrate functionality for a desired amount of time.

## Getting Started

To access the **Vibration** functionality the following platform specific setup is required.

# [Android](#tab/android)

The Vibrate permission is required and must be configured in the Android project. This can be added in the following ways:

Open the **AssemblyInfo.cs** file under the **Properties** folder and add:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

OR Update Android Manifest:

Open the **AndroidManifest.xml** file under the **Properties** folder and add the following inside of the **manifest** node.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Or right click on the Anroid project and open the project's properties. Under **Android Manifest** find the **Required permissions:** area and check the **VIBRATE** permission. This will automatically update the **AndroidManifest.xml** file.

# [iOS](#tab/ios)

No additional setup required.

# [UWP](#tab/uwp)

No additional setup required.

-----

## Using Vibration

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Vibration functionality can be requested for a set amount of time or the default of 500 milliseconds.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Cancellation of device vibration can be requested with the `Cancel` method:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## Platform Differences

| Platform | Difference |
| --- | --- |
| iOS | Always vibrates for 500 milliseconds. |
| iOS | Not possible to cancel vibration. |

## API

- [Vibration source code](https://github.com/xamarin/Essentials/tree/master/Essentials/Vibration)
- [Vibration API documentation](xref:Xamarin.Essentials.Vibration)
