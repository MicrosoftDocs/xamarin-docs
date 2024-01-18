---
title: "Xamarin.Forms Shapes: Rectangle"
description: "The Xamarin.Forms Rectangle class can be used to draw rectangles."
ms.service: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Rectangle

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

The `Rectangle` class derives from the `Shape` class, and can be used to draw rectangles and squares. For information on the properties that the `Rectangle` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

`Rectangle` defines the following properties:

- `RadiusX`, of type `double`, which is the x-axis radius that's used to round the corners of the rectangle. The default value of this property is 0.0.
- `RadiusY`, of type `double`, which is the y-axis radius that's used to round the corners of the rectangle. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The `Rectangle` class sets the `Aspect` property, inherited from the `Shape` class, to `Stretch.Fill`. For more information about the `Aspect` property, see [Stretch shapes](index.md#stretch-shapes).

## Create a Rectangle

To draw a rectangle, create a `Rectangle` object and sets its `WidthRequest` and `HeightRequest` properties. To paint the inside of the rectangle, set its `Fill` property to a [`Brush`](xref:Xamarin.Forms.Brush)-derived object. To give the rectangle an outline, set its `Stroke` property to a [`Brush`](xref:Xamarin.Forms.Brush)-derived object. The `StrokeThickness` property specifies the thickness of the rectangle outline. For more information about `Brush` objects, see [Xamarin.Forms Brushes](~/xamarin-forms/user-interface/brushes/index.md).

To give the rectangle rounded corners, set its `RadiusX` and `RadiusY` properties. These properties set the x-axis and y-axis radii that's used to round the corners of the rectangle.

To draw a square, make the `WidthRequest` and `HeightRequest` properties of the `Rectangle` object equal.

The following XAML example shows how to draw a filled rectangle:

```xaml
<Rectangle Fill="Red"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

In this example, a red filled rectangle with dimensions 150x50 (device-independent units) is drawn:

![Filled rectangle](rectangle-images/filled.png "Filled rectangle")

The following XAML example shows how to draw a filled rectangle, with rounded corners:

```xaml
<Rectangle Fill="Blue"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10"
           WidthRequest="200"
           HeightRequest="100"
           HorizontalOptions="Start" />
```

In this example, a blue filled rectangle with rounded corners is drawn:

![Rectangle with rounded corners](rectangle-images/rounded.png "Rectangle with rounded corners")

For information about drawing a dashed rectangle, see [Draw dashed shapes](index.md#draw-dashed-shapes).

## Related links

- [ShapeDemos (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Shapes](index.md)
- [Xamarin.Forms Brushes](~/xamarin-forms/user-interface/brushes/index.md)
