---
title: "Colors in Xamarin.Forms"
description: "The Xamarin.Forms Color structure lets you specify colors as RGB values, HSL values, HSV values, or with a color name."
ms.service: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Colors in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithcolors)

The [`Color`](xref:Xamarin.Forms.Color) structure lets you specify colors as Red-Green-Blue (RGB) values, Hue-Saturation-Luminosity (HSL) values, Hue-Saturation-Value (HSV) values, or with a color name. An Alpha channel is also available to indicate transparency.

[`Color`](xref:Xamarin.Forms.Color) objects can be created with the `Color` constructors, which can be used to specify a [gray shade](xref:Xamarin.Forms.Color.%23ctor(System.Double)), an [RGB value](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)), or an [RGB value with transparency](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)). In all cases, arguments are `double` values ranging from 0 to 1.

You can also use static methods to create [`Color`](xref:Xamarin.Forms.Color) objects:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) for `double` RGB values from 0 to 1.
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) for integer RGB values from 0 to 255.
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) for `double` RGB values with transparency.
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) for integer RGB values with transparency.
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) for `double` HSL values with transparency.
- [`Color.FromHsv`](xref:Xamarin.Forms.Color.FromHsv(System.Double,System.Double,System.Double)) for `double` HSV values from 0 to 1.
- [`Color.FromHsv`](xref:Xamarin.Forms.Color.FromHsv(System.Int32,System.Int32,System.Int32)) for integer HSV values from 0 to 255.
- [`Color.FromHsva`](xref:Xamarin.Forms.Color.FromHsva(System.Double,System.Double,System.Double,System.Double)) for `double` HSV values with transparency.
- [`Color.FromHsva`](xref:Xamarin.Forms.Color.FromHsva(System.Int32,System.Int32,System.Int32,System.Int32)) for integer HSV values with transparency.
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) for a `uint` value calculated as (B + 256 \* (G + 256 \* (R + 256 \* A))).
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) for a `string` format of hexadecimal digits in the form "#AARRGGBB" or "#RRGGBB" or "#ARGB" or "#RGB", where each letter corresponds to a hexadecimal digit for the alpha, red, green, and blue channels.

Once created, a [`Color`](xref:Xamarin.Forms.Color) object is immutable. The characteristics of the color can be obtained from the following properties:

- [`R`](xref:Xamarin.Forms.Color.R), which represents the red channel of the color.
- [`G`](xref:Xamarin.Forms.Color.G), which represents the green channel of the color.
- [`B`](xref:Xamarin.Forms.Color.B), which represents the blue channel of the color.
- [`A`](xref:Xamarin.Forms.Color.A), which represents the alpha channel of the color.
- [`Hue`](xref:Xamarin.Forms.Color.Hue), which represents the hue channel of the color.
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation), which represents the saturation channel of the color.
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity), which represents the luminosity channel of the color.

These properties are all `double` values ranging from 0 to 1.

## Named colors

The [`Color`](xref:Xamarin.Forms.Color) structure also defines 240 public static read-only fields for common colors, such as [`AliceBlue`](xref:Xamarin.Forms.Color.AliceBlue).

## Color.Accent

The [`Color.Accent`](xref:Xamarin.Forms.Color.Accent) value results in a platform-specific (and sometimes user-selectable) color that is visible on either a dark or light background.

## Color.Default

The [`Color.Default`](xref:Xamarin.Forms.Color.Default) value defines a `Color` with all channels set to -1, and is intended to enforce the platform's color scheme. Consequently, it has a different meaning in different contexts on different platforms. By default the platform color schemes are:

- iOS: dark text on a light background.
- Android: dark text on a light background.
- Windows: dark text on a light background.

## Color.Transparent

The [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent) value defines a `Color` with all channels set to zero.

## Modify a color

Several instance methods allow modifying an existing color to create a new color:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double)) returns a `Color` by modifying the luminosity by the supplied delta.
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double)) returns a `Color` by modifying the alpha, multiplying it by the supplied alpha value.
- [`ToHex`](xref:Xamarin.Forms.Color.ToHex*) returns a hexadecimal `string` representation of a `Color`.
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double)) returns a `Color`, replacing the hue with the value supplied.
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double)) returns a `Color`, replacing the luminosity with the value supplied.
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double)) returns a `Color`, replacing the saturation with the value supplied.

## Implicit conversions

Implicit conversion between the [`Xamarin.Forms.Color`](xref:Xamarin.Forms.Color) and `System.Drawing.Color` types can be performed:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## Examples

In XAML, colors are typically referenced using their named values, or with their Hex representations:

```xaml
<Label Text="Sea color"
       TextColor="Aqua" />
<Label Text="RGB"
       TextColor="#00FF00" />
<Label Text="Alpha plus RGB"
       TextColor="#CC00FF00" />
<Label Text="Tiny RGB"
       TextColor="#0F0" />
<Label Text="Tiny Alpha plus RGB"
       TextColor="#C0F0" />
```

> [!NOTE]
> When using XAML compilation, color names are case insensitive and therefore can be written in lowercase. For more information about XAML compilation, see [XAML Compilation](~/xamarin-forms/xaml/xamlc.md).

In C#, colors are typically referenced using their named values, or with their static methods:

```csharp
Label red    = new Label { Text = "Red",    TextColor = Color.Red };
Label orange = new Label { Text = "Orange", TextColor = Color.FromHex("FF6A00") };
Label yellow = new Label { Text = "Yellow", TextColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
Label green  = new Label { Text = "Green",  TextColor = Color.FromRgb (38, 127, 0) };
Label blue   = new Label { Text = "Blue",   TextColor = Color.FromRgba(0, 38, 255, 255) };
Label indigo = new Label { Text = "Indigo", TextColor = Color.FromRgb (0, 72, 255) };
Label violet = new Label { Text = "Violet", TextColor = Color.FromHsla(0.82, 1, 0.25, 1) };
```

The following example uses the `OnPlatform` markup extension to selectively set the color of an `ActivityIndicator`:

```xaml
<ActivityIndicator Color="{OnPlatform iOS=Black, Default=Default}"
                   IsRunning="True" />
```

The equivalent C# code is:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## Related links

- [ColorsSample](/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Bindable Picker (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
