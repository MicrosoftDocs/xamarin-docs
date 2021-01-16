---
title: "Xamarin.Forms Brushes"
description: "The Xamarin.Forms Brush class is an abstract class that paints an area with its output."
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Brushes

A brush enables you to paint an area, such as the background of a control, using different approaches. Brush support in Xamarin.Forms is available in the `Xamarin.Forms` namespace on iOS, Android, macOS, the Universal Windows Platform (UWP), and the Windows Presentation Foundation (WPF).

The `Brush` class is an abstract class that paints an area with its output. Classes that derive from `Brush` describe different ways of painting an area. The following list describes the different brush types available in Xamarin.Forms:

- `SolidColorBrush`, which paints an area with a solid color. For more information, see [Xamarin.Forms Brushes: Solid colors](solidcolor.md).
- `LinearGradientBrush`, which paints an area with a linear gradient. For more information, see [Xamarin.Forms Brushes: Linear gradients](lineargradient.md).
- `RadialGradientBrush`, which paints an area with a radial gradient. For more information, see [Xamarin.Forms Brushes: Radial gradients](radialgradient.md).

Instances of these brush types can be assigned to the `Stroke` and `Fill` properties of a `Shape`, and the `Background` property of a [`VisualElement`](xref:Xamarin.Forms.VisualElement).

> [!NOTE]
> The `VisualElement.Background` property enables brushes to be used as the background in any control.

The `Brush` class also has an `IsNullOrEmpty` method that returns a `bool` that represents whether the brush is defined or not.
