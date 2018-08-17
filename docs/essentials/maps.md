---
title: "Xamarin.Essentials Maps"
description: "The Maps class in Xamarin.Essentials enables an application to open the installed maps application to a specific location or placemark."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
---

# Xamarin.Essentials: Maps

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Maps** class enables an application to open the installed maps application to a specific location or placemark.

## Using Maps

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Maps functionality works by calling the `OpenAsync` method with the `Location` or `Placemark` to open with optional `MapsLaunchOptions`.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

When opening with a `Placemark`, the following information is required:

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## Extension Methods

If you already have a reference to a `Location` or `Placemark`, you can use the built-in extension method `OpenMapsAsync` with optional `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## Directions Mode

If you call `OpenMapsAsync` without any `MapsLaunchOptions`, the map will launch to the location specified. Optionally, you can have a navigation route calculated from the device's current position. This is accomplished by setting the `MapDirectionsMode` on the `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { MapDirectionsMode = MapDirectionsMode.Driving };

        await Maps.OpenAsync(location, options);
    }
}
```

## Platform Differences

# [Android](#tab/android)

- `MapDirectionsMode` supports Bicycling, Driving, and Walking.

# [iOS](#tab/ios)

- `MapDirectionsMode` supports Driving, Transit, and Walking.

# [UWP](#tab/uwp)

- `MapDirectionsMode` supports Driving, Transit, and Walking.

--------------

## Platform Implementation Specifics

# [Android](#tab/android)

Android uses the `geo:` Uri scheme to launch the maps application on the device. This may prompt the user to select from an existing app that supports this Uri scheme.  Xamarin.Essentials is tested with Google Maps, which supports this scheme.

# [iOS](#tab/ios)

No platform-specific implementation details.

# [UWP](#tab/uwp)

No platform-specific implementation details.

--------------

## API

- [Maps source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps API documentation](xref:Xamarin.Essentials.Maps)
