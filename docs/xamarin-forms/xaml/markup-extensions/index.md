---
title: "XAML Markup Extensions"
description: "The article explains how to use Xamarin.Forms XAML markup extensions to extend the power and flexibility of XAML by allowing element attributes to be set from sources other than literal text strings."
ms.service: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# XAML Markup Extensions

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML markup extensions help extend the power and flexibility of XAML by allowing element attributes to be set from sources other than literal text strings.

For example, normally you set the `Color` property of `BoxView` like this:

```xaml
<BoxView Color="Blue" />
```

Or, you can set it to a hexadecimal RGB color value:

```xaml
<BoxView Color="#FF0080" />
```

In either case, the text string set to the `Color` attribute is converted to a `Color` value by the [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) class.

You might prefer instead to set the `Color` attribute from a value stored in a resource dictionary, or from the value of a static property of a class that you've created, or from a property of type `Color` of another element on the page, or constructed from separate hue, saturation, and luminosity values.

All these options are possible using XAML markup extensions. But don't let the phrase "markup extensions" scare you: XAML markup extensions are *not* extensions to XML. Even with XAML markup extensions, XAML is always legal XML.

A markup extension is really just a different way to express an attribute of an element. XAML markup extensions are usually identifiable by an attribute setting that is enclosed in curly braces:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Any attribute setting in curly braces is *always* a XAML markup extension. However, as you'll see, XAML markup extensions can also be referenced without the use of curly braces.

This article is divided in two parts:

## [Consuming XAML Markup Extensions](consuming.md)  

Use the XAML markup extensions defined in Xamarin.Forms.

## [Creating XAML Markup Extensions](creating.md)

Write your own custom XAML markup extensions.

## Related Links

- [Markup Extensions (sample)](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML markup extensions chapter from Xamarin.Forms book](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamic Styles](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)