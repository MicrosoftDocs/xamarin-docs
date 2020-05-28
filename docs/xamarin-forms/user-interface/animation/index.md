---
title: "Animation in Xamarin.Forms"
description: "Xamarin.Forms includes its own animation infrastructure that's straightforward for creating simple animations, while also being versatile enough to create complex animations."
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Animation in Xamarin.Forms

_Xamarin.Forms includes its own animation infrastructure that's straightforward for creating simple animations, while also being versatile enough to create complex animations._

The Xamarin.Forms animation classes target different properties of visual elements, with a typical animation progressively changing a property from one value to another over a period of time. Note that there is no XAML interface for the Xamarin.Forms animation classes. However, animations can be encapsulated in [behaviors](~/xamarin-forms/app-fundamentals/behaviors/index.md) and then referenced from XAML.

## [Simple Animations](simple.md)

The [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class provides extension methods that can be used to construct simple animations that rotate, scale, translate, and fade [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances. This article demonstrates creating and canceling animations using the `ViewExtensions` class.

## [Easing Functions](easing.md)

Xamarin.Forms includes an [`Easing`](xref:Xamarin.Forms.Easing) class that allows you to specify a transfer function that controls how animations speed up or slow down as they're running. This article demonstrates how to consume the pre-defined easing functions, and how to create custom easing functions.

## [Custom Animations](custom.md)

The [`Animation`](xref:Xamarin.Forms.Animation) class is the building block of all Xamarin.Forms animations, with the extension methods in the [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class creating one or more `Animation` objects. This article demonstrates how to use the `Animation` class to create and cancel animations, synchronize multiple animations, and create custom animations that animate properties that aren't animated by the existing animation methods.
