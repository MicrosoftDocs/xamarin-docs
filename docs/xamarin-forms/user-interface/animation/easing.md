---
title: "Easing Functions in Xamarin.Forms"
description: "Xamarin.Forms includes an Easing class that allows you to specify a transfer function that controls how animations speed up or slow down as they're running. This article demonstrates how to consume the pre-defined easing functions, and how to create custom easing functions."
ms.service: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Easing Functions in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin.Forms includes an Easing class that allows you to specify a transfer function that controls how animations speed up or slow down as they're running. This article demonstrates how to consume the pre-defined easing functions, and how to create custom easing functions._

The [`Easing`](xref:Xamarin.Forms.Easing) class defines a number of easing functions that can be consumed by animations:

- The [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) easing function bounces the animation at the beginning.
- The [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut) easing function bounces the animation at the end.
- The [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn) easing function slowly accelerates the animation.
- The [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) easing function accelerates the animation at the beginning, and decelerates the animation at the end.
- The [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) easing function quickly decelerates the animation.
- The [`Linear`](xref:Xamarin.Forms.Easing.Linear) easing function uses a constant velocity, and is the default easing function.
- The [`SinIn`](xref:Xamarin.Forms.Easing.SinIn) easing function smoothly accelerates the animation.
- The [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut) easing function smoothly accelerates the animation at the beginning, and smoothly decelerates the animation at the end.
- The [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) easing function smoothly decelerates the animation.
- The [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) easing function causes the animation to very quickly accelerate towards the end.
- The [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) easing function causes the animation to quickly decelerate towards the end.

The `In` and `Out` suffixes indicate if the effect provided by the easing function is noticeable at the beginning of the animation, at the end, or both.

In addition, custom easing functions can be created. For more information, see [Custom Easing Functions](#custom-easing-functions).

## Consuming an Easing Function

The animation extension methods in the [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class allow an easing function to be specified as the final method parameter, as demonstrated in the following code example:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

By specifying an easing function for an animation, the animation velocity becomes non-linear and produces the effect provided by the easing function. Omitting an easing function when creating an animation causes the animation to use the default [`Linear`](xref:Xamarin.Forms.Easing.Linear) easing function, which produces a linear velocity.

> [!NOTE]
> Xamarin.Forms 5.0 includes a type converter that converts a string representation of an easing function to the appropriate [`Easing`](xref:Xamarin.Forms.Easing) enumeration member. This type converter is automatically invoked on any properties of type `Easing` that are set in XAML.

For more information about using the animation extension methods in the [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class, see [Simple Animations](~/xamarin-forms/user-interface/animation/simple.md). Easing functions can also be consumed by the [`Animation`](xref:Xamarin.Forms.Animation) class. For more information, see [Custom Animations](~/xamarin-forms/user-interface/animation/custom.md).

## Custom Easing Functions

There are three main approaches to creating a custom easing function:

1. Create a method that takes a `double` argument, and returns a `double` result.
1. Create a `Func<double, double>`.
1. Specify the easing function as the argument to the [`Easing`](xref:Xamarin.Forms.Easing) constructor.

In all three cases, the custom easing function should return 0 for an argument of 0, and 1 for an argument of 1. However, any value can be returned between the argument values of 0 and 1. Each approach will now be discussed in turn.

### Custom Easing Method

A custom easing function can be defined as a method that takes a `double` argument, and returns a `double` result, as demonstrated in the following code example:

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

The `CustomEase` method truncates the incoming value to the values 0, 0.2, 0.4, 0.6, 0.8, and 1. Therefore, the [`Image`](xref:Xamarin.Forms.Image) instance is translated in discrete jumps, rather than smoothly.

### Custom Easing Func

A custom easing function can also be defined as a `Func<double, double>`, as demonstrated in the following code example:

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

The `CustomEaseFunc` represents an easing function that starts off fast, slows down and reverses course, and then reverses course again to accelerate quickly towards the end. Therefore, while the overall movement of the [`Image`](xref:Xamarin.Forms.Image) instance is downwards, it also temporarily reverses course halfway through the animation.

### Custom Easing Constructor

A custom easing function can also be defined as the argument to the [`Easing`](xref:Xamarin.Forms.Easing) constructor, as demonstrated in the following code example:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

The custom easing function is specified as a lambda function argument to the [`Easing`](xref:Xamarin.Forms.Easing) constructor, and uses the `Math.Cos` method to create a slow drop effect that's dampened by the `Math.Exp` method. Therefore, the [`Image`](xref:Xamarin.Forms.Image) instance is translated so that it appears to drop to its final resting place.

## Summary

This article demonstrated how to consume the pre-defined easing functions, and how to create custom easing functions. Xamarin.Forms includes an [`Easing`](xref:Xamarin.Forms.Easing) class that allows you to specify a transfer function that controls how animations speed up or slow down as they're running.

## Related Links

- [Async Support Overview](~/cross-platform/platform/async.md)
- [Easing Functions (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [Easing](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)