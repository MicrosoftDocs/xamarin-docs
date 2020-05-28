---
title: "Xamarin.Forms Map"
description: "The Map control displays a map, and requires the Xamarin.Forms.Maps NuGet package."
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Map

## [Initialization and Configuration](setup.md)

The [Xamarin.Forms.Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet package is required to use maps functionality in an application. In addition, accessing the user's location requires location permissions to have been granted to the application.

## [Map Control](map.md)

The [`Map`](xref:Xamarin.Forms.Maps.Map) control is a cross-platform view for displaying and annotating maps. It uses the native map control for each platform, providing a fast and familiar maps experience for users.

## [Position and Distance](position-distance.md)

The [`Position`](xref:Xamarin.Forms.Maps.Position) struct is typically used when positioning a map and its pins, and the [`Distance`](xref:Xamarin.Forms.Maps.Distance) struct that can optionally be used when positioning a map.

## [Pins](pins.md)

The [`Map`](xref:Xamarin.Forms.Maps.Map) control allows locations to be marked with [`Pin`](xref:Xamarin.Forms.Maps.Pin) objects. A `Pin` is a map marker that opens an information window when tapped.

## [Polygons, Polylines, and Circles](polygons.md)

`Polygon`, `Polyline`, and `Circle` elements allow you to highlight specific areas on a map. A `Polygon` is a fully enclosed shape that can have a stroke and fill color. A `Polyline` is a line that does not fully enclose an area. A `Circle` highlights a circular area of the map.

## [Geocoding](geocoder.md)

The [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) class converts between string addresses and latitude and longitude coordinates that are stored in [`Position`](xref:Xamarin.Forms.Maps.Position) objects.

## [Launch the Native Map App](native-map-app.md)

The native map app on each platform can be launched from a Xamarin.Forms application by the Xamarin.Essentials `Launcher` class.
