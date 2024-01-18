---
title: "Xamarin.Forms Map Geocoding"
description: "This article explains how to geocode and reverse geocode map data, using the Xamarin.Forms.Maps Geocoder class."
ms.service: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Map Geocoding

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithmaps)

The [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) namespace provides a [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) class, which converts between string addresses and latitude and longitude coordinates that are stored in [`Position`](xref:Xamarin.Forms.Maps.Position) objects. For more information about the [`Position`](xref:Xamarin.Forms.Maps.Position) struct, see [Map Position and Distance](position-distance.md).

> [!NOTE]
> An alternative geocoding API is available in Xamarin.Essentials. The Xamarin.Essentials `Geocoding` API offers structured address data when geocoding addresses, as opposed to the strings returned by this API. For more information, see [Xamarin.Essentials: Geocoding](~/essentials/geocoding.md).

## Geocode an address

A street address can be geocoded into latitude and longitude coordinates by creating a [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) instance and calling the [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) method on the `Geocoder` instance:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

The [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) method takes a `string` argument that represents the address, and asynchronously returns a collection of [`Position`](xref:Xamarin.Forms.Maps.Position) objects that could represent the address.

## Reverse geocode an address

Latitude and longitude coordinates can be reverse geocoded into a street address by creating a [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) instance and calling the [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) method on the `Geocoder` instance:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

The [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) method takes a [`Position`](xref:Xamarin.Forms.Maps.Position) argument comprised of latitude and longitude coordinates, and asynchronously returns a collection of strings that represent the addresses near the position.

## Related links

- [Maps Sample](/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Forms Map Position and Distance](position-distance.md)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)
