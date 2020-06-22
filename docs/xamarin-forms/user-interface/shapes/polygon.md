---
title: "Xamarin.Forms Shapes: Polygon"
description: "The Xamarin.Forms Polygon class can be used to draw polygons, which are connected series of lines that form closed shapes."
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Polygon

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

The `Polygon` class derives from the `Shape` class, and can be used to draw polygons, which are connected series of lines that form closed shapes. For information on the properties that the `Polygon` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

`Polygon` defines the following properties:

- `Points`, of type `PointCollection`, which is a collection of `Point` structures that describe the vertex points of the polygon.
- `FillRule`, of type `FillRule`, which specifies how the interior fill of the shape is determined. The default value of this property is `FillRule.EvenOdd`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Create a Polygon

The following XAML example shows how to draw a polygon:

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## Related links

- [ShapeDemos (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Shapes](index.md)
