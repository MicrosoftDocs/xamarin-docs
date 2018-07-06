---
title: "Summary of Chapter 3. Deeper into text"
description: "Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 3. Deeper into text"
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
---

# Summary of Chapter 3. Deeper into text

This chapter explores the [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) view in more depth, including color, fonts, and formatting.

## Wrapping paragraphs

When the [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) property of `Label` contains long text, `Label` automatically wraps it to multiple lines as demonstrated by the [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) sample. You can embed Unicode codes such as '\u2014' for the em-dash, or C# characters like '\r' to break to a new line.

When the [`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) and [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) properties of a `Label` are set to `LayoutOptions.Fill`, the overall size of the `Label` is governed by the space that its container makes available. The `Label` is said to be *constrained*. The size of the `Label` is the size of its container.

When the `HorizontalOptions` and `VerticalOptions` properties are set to values other than `LayoutOptions.Fill`, the size of the `Label` is governed by the space required to render the text, up to the size that its container makes available for the `Label`. The `Label` is said to be *unconstrained* and it determines its own size.

(Note: The terms *constrained* and *unconstrained* might be counter-intuitive, because an unconstrained view is generally smaller than a constrained view. Also, these terms are not used consistently in the early chapters of the book.)

A view such as a `Label` can be constrained in one dimension and unconstrained in the other. A `Label` will only wrap text on multiple lines if it is constrained horizontally.

If a `Label` is constrained, it might occupy considerably more space than required for the text. The text can be positioned within the overall area of the `Label`. Set the [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) property to a member of the [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) enumeration ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center), or [`End`](xref:Xamarin.Forms.TextAlignment.Center)) to control the alignment of all the lines of the paragraph. The default is `Start` and left-aligns the text.

Set the [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) property to a member of the `TextAlignment` enumeration to position the text at the top, center, or bottom of the area occupied by the `Label`.

Set the [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) property to a member of the [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) enumeration ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), or [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) to control how the multiple lines in a paragraph break or are truncated.

## Text and background colors

Set the [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) and [`BackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) properties of `Label` to [`Color`](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) values to control the color of the text and background.

The `BackgroundColor` applies to the background of the entire area occupied by the `Label`. Depending on the `HorizontalOptions` and `VerticalOptions` properties, that size might be considerably larger than the area required to display the text. You can use color to experiment with various values of `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, and `VerticalTextAlignment` to see how they affect the size and position of the `Label`, and the size and position of the text within the `Label`.

## The Color structure

The [`Color`](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) structure lets you specify colors as Red-Green-Blue (RGB) values, or Hue-Saturation-Luminosity (HSL) values, or with a color name. An Alpha channel is also available to indicate transparency.

Use a `Color` constructor to specify:

- a [gray shade](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- an [RGB value](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- an [RGB value with transparency](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Arguments are `double` values ranging from 0 to 1.

You can also use several static methods to create `Color` values:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) for `double` RGB values from 0 to 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) for integer RGB values from 0 to 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) for `double` RGB values with transparency
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) for integer RGB values with transparency
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) for `double` HSL values with transparency
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) for a `uint` value calculated as (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) for a `string` format of hexadecimal digits in the form "#AARRGGBB" or "#RRGGBB" or "#ARGB" or "#RGB", where each letter corresponds to a hexadecimal digit for the alpha, red, green, and blue channels. This method is primary used for XAML color conversions as discussed in [Chapter 7, XAML vs. code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Once created, a `Color` value is immutable. The characteristics of the color can be obtained from the following properties:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

These are all `double` values ranging from 0 to 1.

`Color` also defines 240 public static read-only fields for common colors. At the time the book was written, only 17 common colors were available.

Another public static read-only field defines a color with all color channels set to zero:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Several instance methods allow modifying an existing color to create a new color:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Finally, two static read-only properties define special color value:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), all channels set to &ndash;1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` is intended to enforce the platform's color scheme, and consequently has a different meaning in different contexts on different platforms. By default the platform color schemes are:

- iOS: Dark text on a light background
- Android: Light text on a dark background (in the book) or dark text on a light background (for Material Design via AppCompat in the **master** branch of the sample code repository)
- UWP: Dark text on a light background
- Windows 8.1: Light text on a dark background
- Windows Phone 8.1: Light text on a dark background

The `Color.Accent` value results in a platform-specific (and sometimes user-selectable) color that is visible on either a dark or light background.

## Changing the application color scheme

The various platforms have a default color scheme as shown in the list above.

When targeting Android, it's possible to switch to a dark-on-light scheme by specifying a light theme in the Android.Manifest.xml file, or by
[Adding AppCompat and Material Design](~/xamarin-forms/platform/android/appcompat.md).

For the Windows platforms, the color theme is normally selected by the user, but you can add a `RequestedTheme` attribute set to either `Light` or `Dark` in the platform's App.xaml file. By default, the App.xaml file in the UWP project contains a `RequestedTheme` attribute set to `Light`.

## Font sizes and attributes

Set the [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) property of `Label` to a string such as "Times Roman" to select a font family. However, you need to specify a font family that is supported on the particular platform, and the platforms are inconsistent in this regard.

Set the [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) property of `Label` to a `double` for specifying the approximate height of the font. See [Chapter 5, Dealing with Sizes](chapter05.md), for more details on intelligently choosing font sizes.

You can alternatively obtain one of several preset platform-dependent font sizes. The static [`Device.GetNamedSize`](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) method and [overload](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) both return a `double` font size value appropriate to the platform based on members of the [`NamedSize`](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)  enumeration ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small), [`Medium`](xref:Xamarin.Forms.NamedSize.Medium), and [`Large`](xref:Xamarin.Forms.NamedSize.Large)). The value returned from the `Medium` member is not necessarily the same as `Default`. The [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) sample displays text with these named sizes.

Set the [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) property of `Label` to a member of these [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) enumeration, [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold),  [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic), or [`None`](xref:Xamarin.Forms.FontAttributes.None). You can combine the `Bold` and `Italic` members with the C# bitwise OR operator.

## Formatted text

In all of the examples so far, the entire text displayed by the `Label` has been formatted uniformly. To vary the formatting within a text string, don't set the `Text` property of `Label`. Instead, set the [`FormattedText`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) property to an object of type [`FormattedString`](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` has a [`Spans`](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) property that is a collection of [`Span`](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) objects. Each `Span` object has its own [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [`ForegroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), and [`BackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) properties.

The [**VariableFormattedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) sample demonstrates using the `FormattedText` property for a single line of text, and [**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) demonstrates the technique for an entire paragraph, as shown here:

[![Triple screenshot of variable formatted paragraph](images/ch03fg06-small.png "Variable Formatted Label Text")](images/ch03fg06-large.png#lightbox "Variable Formatted Label Text")

The [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program uses a single `Label` and a `FormattedString` object to display all of the named font sizes for each platform.



## Related Links

- [Chapter 3 full text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Chapter 3 samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Chapter 3 F# samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [Working with Colors](~/xamarin-forms/user-interface/colors.md)
