---
title: "Xamarin.Essentials Geocoding"
description: "The Geocoding class provides APIs to geocode a placemark to a positional coordinates and reverse geocode coordincates to a placemark."
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Geocoding

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Geocoding** class provides APIs to geocode a placemark to a positional coordinates and reverse geocode coordincates to a placemark.

## Getting Started

To access the **Geocoding** functionality the following platform specific setup is required.

# [Android](#tab/android)

No additional setup required.

# [iOS](#tab/ios)

No additional setup required.

# [UWP](#tab/uwp)

A Bing maps API key is required to use geocoding funcationality. Sign up for a free [Bing Maps](https://www.bingmapsportal.com/) account. Under **My account > My keys** create a new key and fill out information based on your application type.

Early on in your application's life before calling any **Geocoding** methods set the API key:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## Using Geocoding

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

Getting [location](xref:Xamarin.Essentials.Location) coordinates for an address:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

Getting [placemarks](xref:Xamarin.Essentials.Placemark) for an existing set of coordinates:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## API

- [Geocoding source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Geocoding API documentation](xref:Xamarin.Essentials.Geocoding)
