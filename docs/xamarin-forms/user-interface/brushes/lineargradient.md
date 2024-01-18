---
title: "Xamarin.Forms Brushes: Linear gradients"
description: "The Xamarin.Forms LinearGradientBrush class paints an area with a linear gradient."
ms.service: xamarin
ms.assetid: BEA2B3F5-96B0-4E39-88A6-0FAFE95C3DCD
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Brushes: Linear gradients

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

The `LinearGradientBrush` class derives from the `GradientBrush` class, and paints an area with a linear gradient, which blends two or more colors along a line known as the gradient axis. `GradientStop` objects are used to specify the colors in the gradient and their positions. For more information about `GradientStop` objects, see [Xamarin.Forms Brushes: Gradients](gradient.md).

The `LinearGradientBrush` class defines the following properties:

- `StartPoint`, of type [`Point`](xref:Xamarin.Forms.Point), which represents the starting two-dimensional coordinates of the linear gradient. The default value of this property is (0,0).
- `EndPoint`, of type [`Point`](xref:Xamarin.Forms.Point), which represents the ending two-dimensional coordinates of the linear gradient. The default value of this property is (1,1).

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The `LinearGradientBrush` class also as an `IsEmpty` method that returns a `bool` that represents whether the brush has been assigned any `GradientStop` objects.

> [!NOTE]
> Linear gradients can also be created with the `linear-gradient()` CSS function.

## Create a LinearGradientBrush

A linear gradient brush's gradient stops are positioned along the gradient axis. The orientation and size of the gradient axis can be changed using the brush's `StartPoint` and `EndPoint` properties. By manipulating these properties, you can create horizontal, vertical, and diagonal gradients, reverse the gradient direction, condense the gradient spread, and more.

The `StartPoint` and `EndPoint` properties are relative to the area being painted. (0,0) represents the top-left corner of the area being painted, and (1,1) represents the bottom-right corner of the area being painted. The following diagram shows the gradient axis for a diagonal linear gradient brush:

![Frame with a diagonal gradient axis](lineargradient-images/gradient-axis.png)

In this diagram, the dashed line shows the gradient axis, which highlights the interpolation path of the gradient from the start point to the end point.

### Create a horizontal linear gradient

To create a horizontal linear gradient, create a `LinearGradientBrush` object and set its `StartPoint` to (0,0) and its `EndPoint` to (1,0). Then, add two or more `GradientStop` objects to the `LinearGradientBrush.GradientStops` collection, that specify the colors in the gradient and their positions.

The following XAML example shows a horizontal `LinearGradientBrush` that's set as the `Background` of a [`Frame`](xref:Xamarin.Forms.Frame):

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>  
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a `LinearGradientBrush` that interpolates from yellow to green horizontally:

![Frame painted with a horizontal LinearGradientBrush](lineargradient-images/horizontal.png)

### Create a vertical linear gradient

To create a vertical linear gradient, create a `LinearGradientBrush` object and set its `StartPoint` to (0,0) and its `EndPoint` to (0,1). Then, add two or more `GradientStop` objects to the `LinearGradientBrush.GradientStops` collection, that specify the colors in the gradient and their positions.

The following XAML example shows a vertical `LinearGradientBrush` that's set as the `Background` of a [`Frame`](xref:Xamarin.Forms.Frame):

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->    
        <LinearGradientBrush EndPoint="0,1">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a `LinearGradientBrush` that interpolates from yellow to green vertically:

![Frame painted with a vertical LinearGradientBrush](lineargradient-images/vertical.png)

### Create a diagonal linear gradient

To create a diagonal linear gradient, create a `LinearGradientBrush` object and set its `StartPoint` to (0,0) and its `EndPoint` to (1,1). Then, add two or more `GradientStop` objects to the `LinearGradientBrush.GradientStops` collection, that specify the colors in the gradient and their positions.

The following XAML example shows a diagonal `LinearGradientBrush` that's set as the `Background` of a [`Frame`](xref:Xamarin.Forms.Frame):

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0)      
             Endpoint defaults to (1,1) -->
        <LinearGradientBrush>
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a `LinearGradientBrush` that interpolates from yellow to green diagonally:

![Frame painted with a diagonal LinearGradientBrush](lineargradient-images/diagonal.png)

## Related links

- [BrushesDemos (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.Forms Brushes: Gradients](gradient.md)
