---
title: "Xamarin.Forms Shapes: Ellipse"
description: "The Xamarin.Forms Ellipse class can be used to draw ellipses and circles."
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Ellipse

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

The `Ellipse` class derives from the `Shape` class, and can be used to draw ellipses and circles. For information on the properties that the `Ellipse` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

The `Ellipse` class sets the `Aspect` property, inherited from the `Shape` class, to `Stretch.Fill`. For more information about the `Aspect` property, see [Stretch shapes](index.md#stretch-shapes).

## Create an Ellipse

To draw an ellipse, create an `Ellipse` object and set its `WidthRequest` and `HeightRequest` properties. To paint the inside of the ellipse, set its `Fill` property to a [`Color`](xref:Xamarin.Forms.Color). To give the ellipse an outline, set its `Stroke` property to a [`Color`](xref:Xamarin.Forms.Color). The `StrokeThickness` property specifies the thickness of the ellipse outline.

To draw a circle, make the `WidthRequest` and `HeightRequest` properties of the `Ellipse` object equal.

The following XAML example shows how to draw a filled ellipse:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

In this example, a red filled ellipse with dimensions 150x50 (device-independent units) is drawn:

![Filled ellipse](ellipse-images/filled.png "Filled ellipse")

The following XAML example shows how to draw a circle:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

In this example, a red circle with dimensions 150x150 (device-independent units) is drawn:

![Circle](ellipse-images/circle.png "Circle")

For information about drawing a dashed ellipse, see [Draw dashed shapes](index.md#draw-dashed-shapes).

## Related links

- [ShapeDemos (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Shapes](index.md)
