---
title: "Xamarin.Forms Brushes: Solid colors"
description: "The Xamarin.Forms SolidColorBrush class paints an area with a solid color."
ms.service: xamarin
ms.assetid: 4225D40A-16C1-40E1-ACBE-23E321E7FDE4
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Brushes: Solid colors

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

The `SolidColorBrush` class derives from the `Brush` class, and is used to paint an area with a solid color. There are a variety of approaches to specifying the color of a `SolidColorBrush`. For example, you can specify its color with a [`Color`](xref:Xamarin.Forms.Color) value or by using one of the predefined `SolidColorBrush` objects provided by the `Brush` class.

The `SolidColorBrush` class defines the `Color` property, of type [`Color`](xref:Xamarin.Forms.Color), which represents the color of the brush. This property is backed by a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) object, which means that it can be the target of data bindings, and styled.

The `SolidColorBrush` class also has an `IsEmpty` method that returns a `bool` that represents whether the brush has been assigned a color.

## Create a SolidColorBrush

There are three main techniques for creating a `SolidColorBrush`. You can create a `SolidColorBrush` from a [`Color`](xref:Xamarin.Forms.Color), use a predefined brush, or create a `SolidColorBrush` using hexadecimal notation.

### Use a predefined Color

Xamarin.Forms includes a type converter that creates a `SolidColorBrush` from a [`Color`](xref:Xamarin.Forms.Color) value. In XAML, this enables a `SolidColorBrush` to be created from a predefined `Color` value:

```xaml
<Frame Background="DarkBlue"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a dark blue `SolidColorBrush`:

![Frame painted with a predefined Color](solidcolor-images/predefined-color.png)

Alternatively, the [`Color`](xref:Xamarin.Forms.Color) value can be specified using property tag syntax:

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
       <Frame.Background>
           <SolidColorBrush Color="DarkBlue" />
       </Frame.Background>
</Frame>
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a `SolidColorBrush` whose color is specified by setting the `SolidColorBrush.Color` property.

### Use a predefined Brush

The `Brush` class defines a set of commonly used `SolidColorBrush` objects. The following example uses one of these predefined `SolidColorBrush` objects:

```xaml
<Frame Background="{x:Static Brush.Indigo}"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />       
```

The equivalent C# code is:

```csharp
Frame frame = new Frame
{
    Background = Brush.Indigo,
    BorderColor = Color.LightGray,
    // ...
};
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with an indigo `SolidColorBrush`:

![Frame painted with a predefined SolidColorBrush](solidcolor-images/predefined-brush.png)

For a list of predefined `SolidColorBrush` objects provided by the `Brush` class, see [Solid color brushes](#solid-color-brushes).

### Use hexadecimal notation

`SolidColorBrush` objects can also be created using hexadecimal notation. With this approach, a color is specified in terms of the amount of red, green, and blue to combine into a single color. The main format for specifying a color using hexadecimal notation is `#rrggbb`, where:

- `rr` is a two-digit hexadecimal number specifying the relative amount of red.
- `gg` is a two-digit hexadecimal number specifying the relative amount of green.
- `bb` is a two-digit hexadecimal number specifying the relative amount of blue.

In addition, a color can be specified as `#aarrggbb` where `aa` specifies the alpha value, or transparency, of the color. This approach enables you to create colors that are partially transparent.

The following example sets the color value of a `SolidColorBrush` using hexadecimal notation:

```xaml
<Frame Background="#FF9988"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

In this example, the background of the [`Frame`](xref:Xamarin.Forms.Frame) is painted with a salmon-colored `SolidColorBrush`:

![Frame painted with a SolidColorBrush created using hexadecimal notation](solidcolor-images/hex.png)

For other ways of describing color, see [Colors in Xamarin.Forms](~/xamarin-forms/user-interface/colors.md).

## Solid color brushes

For convenience, the `Brush` class provides a set of commonly used `SolidColorBrush` objects, such as `AliceBlue` and `YellowGreen`. The following image shows the color of each predefined brush, its name, and its hexadecimal value:

[![Color table including a color swatch, color name, and hexadecimal value](solidcolor-images/solidcolorbrushes.png)](solidcolor-images/solidcolorbrushes-large.png#lightbox)

## Related links

- [BrushesDemos (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Colors in Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)
