---
title: "Xamarin.Forms Shapes: Ellipse"
description: "The Xamarin.Forms Ellipse class can be used to draw ellipses and circles."
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Ellipse

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

The `Ellipse` class derives from the `Shape` class, and can be used to draw ellipses and circles. For information on the properties that the `Ellipse` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

The `Ellipse` class sets the `Aspect` property, inherited from the `Shape` class, to `Stretch.Fill`.

## Create an Ellipse

The following XAML example shows how to draw a filled ellipse:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

The following XAML example shows how to draw a circle:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## Related links

- [ShapeDemos (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms Shapes](index.md)
