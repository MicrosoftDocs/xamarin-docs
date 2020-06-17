---
title: "Xamarin.Forms Shapes"
description: "Xamarin.Forms Shapes are types of Views that enable you to draw shapes to the screen."
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes

![](~/media/shared/preview.png "This API is currently pre-release")

A `Shape` is a type of [`View`](xref:Xamarin.Forms.View) that enables you to draw a shape to the screen. `Shape` objects can be used inside layout classes and most controls, because the `Shape` class derives from the `View` class.

Xamarin.Forms Shapes is available in the `Xamarin.Forms.Shapes` namespace on iOS, Android, macOS, the Universal Windows Platform (UWP), and the Windows Presentation Foundation (WPF).

> [!IMPORTANT]
> Xamarin.Forms Shapes is currently experimental and can only be used by setting the `Shapes_Experimental` flag. For more information, see [Experimental Flags](~/xamarin-forms/internals/experimental-flags.md).

`Shape` defines the following properties:

- `Aspect`, of type `Stretch`, describes how the shape fills its allocated space. The default value of this property is `Stretch.None`.
- `Fill`, of type `Color`, indicates the color used to paint the shape's interior.
- `Stroke`, of type `Color`, indicates the color used to paint the shape's outline.
- `StrokeDashArray`, of type `DoubleCollection`, which represents a collection of `double` values that indicate the pattern of dashes and gaps that are used to outline a shape.
- `StrokeDashOffset`, of type `double`, specifies the distance within the dash pattern where a dash begins. The default value of this property is 0.0.
- `StrokeLineCap`, of type `PenLineCap`, describes the shape at the beginning and end of a line or segment. The default value of this property is `PenLineCap.Flat`.
- `StrokeLineJoin`, of type `PenLineJoin`, specifies the type of join that is used at the vertices of a shape. The default value of this property is `PenLineJoin.Miter`.
- `StrokeThickness`, of type `double`, indicates the width of the shape outline. The default value of this property is 1.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

Xamarin.Forms defines a number of objects that derive from the `Shape` class. These include `Ellipse`, `Line`, `Path`, `Polygon`, `Polyline`, and `Rectangle`.

## Related links

- [ShapeDemos (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
