---
title: "Xamarin.Forms Shapes: Polyline"
description: "The Xamarin.Forms Polyline class can be used to draw a series of connected straight lines."
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Polyline

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

The `Polyline` class derives from the `Shape` class, and can be used to draw a series of connected straight lines. For information on the properties that the `Polyline` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

`Polyline` defines the following properties:

- `Points`, of type `PointCollection`, which is a collection of `Point` structures that describe the vertex points of the polyline.
- `FillRule`, of type `FillRule`, which specifies how the intersecting areas in the polyline are combined. The default value of this property is `FillRule.EvenOdd`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Create a Polyline

The following XAML example shows how to draw a polygon:

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## Related links

- [ShapeDemos (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms Shapes](index.md)
