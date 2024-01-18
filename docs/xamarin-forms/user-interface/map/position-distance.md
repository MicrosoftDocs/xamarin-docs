---
title: "Xamarin.Forms Map Position and Distance"
description: "The Xamarin.Forms.Maps namespace contains a Position struct that's typically used when positioning a map and its pins, and a Distance struct that can optionally be used when positioning a map."
ms.service: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Map Position and Distance

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithmaps)

The [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) namespace contains a [`Position`](xref:Xamarin.Forms.Maps.Position) struct that's typically used when positioning a map and its pins, and a [`Distance`](xref:Xamarin.Forms.Maps.Distance) struct that can optionally be used when positioning a map.

## Position

The [`Position`](xref:Xamarin.Forms.Maps.Position) struct encapsulates a position stored as latitude and longitude values. This struct defines two read-only properties:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude), of type `double`, which represents the latitude of the position in decimal degrees.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude), of type `double`, which represents the longitude of the position in decimal degrees.

[`Position`](xref:Xamarin.Forms.Maps.Position) objects are created with the `Position` constructor, which requires latitude and longitude arguments specified as `double` values:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

When creating a `Position` object, the latitude value will be clamped between -90.0 and 90.0, and the longitude value will be clamped between -180.0 and 180.0.

> [!NOTE]
> The `GeographyUtils` class has a `ToRadians` extension method that converts a `double` value from degrees to radians, and a `ToDegrees` extension method that converts a `double` value from radians to degrees.

## Distance

The [`Distance`](xref:Xamarin.Forms.Maps.Distance) struct encapsulates a distance stored as a `double` value, which represents the distance in meters. This struct defines three read-only properties:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers), of type `double`, which represents the distance in kilometers that's spanned by the `Distance`.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters), of type `double`, which represents the distance in meters that's spanned by the `Distance`.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles), of type `double`, which represents the distance in miles that's spanned by the `Distance`.

[`Distance`](xref:Xamarin.Forms.Maps.Distance) objects can be created with the `Distance` constructor, which requires a meters argument specified as a `double`:

```csharp
Distance distance = new Distance(1450.5);
```

Alternatively, [`Distance`](xref:Xamarin.Forms.Maps.Distance) objects can be created with the [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*), [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*), [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*), and `BetweenPositions` factory methods:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## Related links

- [Maps Sample](/samples/xamarin/xamarin-forms-samples/workingwithmaps)