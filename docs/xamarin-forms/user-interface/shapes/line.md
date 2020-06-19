---
title: "Xamarin.Forms Shapes: Line"
description: "The Xamarin.Forms Line class can be used to draw lines."
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Line

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

The `Line` class derives from the `Shape` class, and can be used to draw lines. For information on the properties that the `Line` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

`Line` defines the following properties:

- `X1`, of type double, indicates the x-coordinate of the start point of the line. The default value of this property is 0.0.
- `X2`, of type double, indicates the x-coordinate of the end point of the line. The default value of this property is 0.0.
- `Y1`, of type double, indicates the y-coordinate of the start point of the line. The default value of this property is 0.0.
- `Y2`, of type double, indicates the y-coordinate of the end point of the line. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Create a Line

The following XAML example shows how to draw a line:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> Setting the `Fill` property of a `Line` has no effect, because a line has no interior.

## Related links

- [ShapeDemos (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms Shapes](index.md)
