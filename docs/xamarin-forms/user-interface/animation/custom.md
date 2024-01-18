---
title: "Custom Animations in Xamarin.Forms"
description: "This article demonstrates how to use the Xamarin.FOrms Animation class to create and cancel animations, synchronize multiple animations, and create custom animations that animate properties that aren't animated by the existing animation methods."
ms.service: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Custom Animations in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_The Animation class is the building block of all Xamarin.Forms animations, with the extension methods in the ViewExtensions class creating one or more Animation objects. This article demonstrates how to use the Animation class to create and cancel animations, synchronize multiple animations, and create custom animations that animate properties that aren't animated by the existing animation methods._

A number of parameters must be specified when creating an `Animation` object, including start and end values of the property being animated, and a callback that changes the value of the property. An `Animation` object can also maintain a collection of child animations that can be run and synchronized. For more information, see [Child Animations](#child-animations).

Running an animation created with the [`Animation`](xref:Xamarin.Forms.Animation) class, which may or may not include child animations, is achieved by calling the [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) method. This method specifies the duration of the animation, and amongst other items, a callback that controls whether to repeat the animation.

In addition, the [`Animation`](xref:Xamarin.Forms.Animation) class has an `IsEnabled` property that can be examined to determine if animations have been disabled by the operating system, such as when power saving mode is activated.

## Create an animation

When creating an [`Animation`](xref:Xamarin.Forms.Animation) object, typically, a minimum of three parameters are required, as demonstrated in the following code example:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

This code defines an animation of the [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) property of an [`Image`](xref:Xamarin.Forms.Image) instance from a value of 1 to a value of 2. The animated value, which is derived by Xamarin.Forms, is passed to the callback specified as the first argument, where it's used to change the value of the `Scale` property.

The animation is started with a call to the [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) method, as demonstrated in the following code example:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Note that the [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) method does not return a `Task` object. Instead, notifications are provided through callback methods.

The following arguments are specified in the `Commit` method:

- The first argument (*owner*) identifies the owner of the animation. This can be the visual element on which the animation is applied, or another visual element, such as the page.
- The second argument (*name*) identifies the animation with a name. The name is combined with the owner to uniquely identify the animation. This unique identification can then be used to determine whether the animation is running ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), or to cancel it ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- The third argument (*rate*) indicates the number of milliseconds between each call to the callback method defined in the [`Animation`](xref:Xamarin.Forms.Animation) constructor.
- The fourth argument (*length*) indicates the duration of the animation, in milliseconds.
- The fifth argument (*easing*) defines the easing function to be used in the animation. Alternatively, the easing function can be specified as an argument to the [`Animation`](xref:Xamarin.Forms.Animation) constructor. For more information about easing functions, see [Easing Functions](~/xamarin-forms/user-interface/animation/easing.md).
- The sixth argument (*finished*) is a callback that will be executed when the animation has completed. This callback takes two arguments, with the first argument indicating a final value, and the second argument being a `bool` that's set to `true` if the animation was canceled. Alternatively, the *finished* callback can be specified as an argument to the [`Animation`](xref:Xamarin.Forms.Animation) constructor. However, with a single animation, if *finished* callbacks are specified in both the `Animation` constructor and the `Commit` method, only the callback specified in the `Commit` method will be executed.
- The seventh argument (*repeat*) is a callback that allows the animation to be repeated. It's called at the end of the animation, and returning `true` indicates that the animation should be repeated.

The overall effect is to create an animation that increases the [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) property of an [`Image`](xref:Xamarin.Forms.Image) from 1 to 2, over 2 seconds (2000 milliseconds), using the [`Linear`](xref:Xamarin.Forms.Easing.Linear) easing function. Each time the animation completes, its `Scale` property is reset to 1 and the animation repeats.

> [!NOTE]
> Concurrent animations, that run independently of each other can be constructed by creating an `Animation` object for each animation, and then calling the `Commit` method on each animation.

### Child animations

The [`Animation`](xref:Xamarin.Forms.Animation) class also supports child animations, which involves creating an `Animation` object to which other `Animation` objects are added. This enables a series of animations to be run and synchronized. The following code example demonstrates creating and running child animations:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Alternatively, the code example can be written more concisely, as demonstrated in the following code example:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

In both code examples, a parent [`Animation`](xref:Xamarin.Forms.Animation) object is created, to which additional `Animation` objects are then added. The first two arguments to the [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) method specify when to begin and finish the child animation. The argument values must be between 0 and 1, and represent the relative period within the parent animation that the specified child animation will be active. Therefore, in this example the `scaleUpAnimation` will be active for the first half of the animation, the `scaleDownAnimation` will be active for the second half of the animation, and the `rotateAnimation` will be active for the entire duration.

The overall effect is that the animation occurs over 4 seconds (4000 milliseconds). The `scaleUpAnimation` animates the [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) property from 1 to 2, over 2 seconds. The `scaleDownAnimation` then animates the `Scale` property from 2 to 1, over 2 seconds. While both scale animations are occurring, the `rotateAnimation` animates the [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) property from 0 to 360, over 4 seconds. Note that the scaling animations also use easing functions. The [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) easing function causes the [`Image`](xref:Xamarin.Forms.Image) to initially shrink before getting larger, and the [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) easing function causes the `Image` to become smaller than its actual size towards the end of the complete animation.

There are a number of differences between an [`Animation`](xref:Xamarin.Forms.Animation) object that uses child animations, and one that doesn't:

- When using child animations, the *finished* callback on a child animation indicates when the child has completed, and the *finished* callback passed to the `Commit` method indicates when the entire animation has completed.
- When using child animations, returning `true` from the *repeat* callback on the `Commit` method will not cause the animation to repeat, but the animation will continue to run without new values.
- When including an easing function in the `Commit` method, and the easing function returns a value greater than 1, the animation will be terminated. If the easing function returns a value less than 0, the value is clamped to 0. To use an easing function that returns a value less than 0 or greater than 1, it must specified in one of the child animations, rather than in the `Commit` method.

The [`Animation`](xref:Xamarin.Forms.Animation) class also includes [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) methods that can be used to add child animations to a parent `Animation` object. However, their *begin* and *finish* argument values aren't restricted to 0 to 1, but only that part of the child animation that corresponds to a range of 0 to 1 will be active. For example, if a `WithConcurrent` method call defines a child animation that targets a [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) property from 1 to 6, but with *begin* and *finish* values of -2 and 3, the *begin* value of -2 corresponds to a `Scale` value of 1, and the *finish* value of 3 corresponds to a `Scale` value of 6. Because values outside the range of 0 and 1 play no part in an animation, the `Scale` property will only be animated from 3 to 6.

## Cancel an animation

An application can cancel an animation with a call to the [`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) extension method, as demonstrated in the following code example:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Note that animations are uniquely identified by a combination of the animation owner, and the animation name. Therefore, the owner and name specified when running the animation must be specified to cancel the animation. Therefore, the code example will immediately cancel the animation named `SimpleAnimation` that's owned by the page.

## Create a custom animation

The examples shown here so far have demonstrated animations that could equally be achieved with the methods in the [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class. However, the advantage of the [`Animation`](xref:Xamarin.Forms.Animation) class is that it has access to the callback method, which is executed when the animated value changes. This allows the callback to implement any desired animation. For example, the following code example animates the [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) property of a page by setting it to [`Color`](xref:Xamarin.Forms.Color) values created by the [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) method, with hue values ranging from 0 to 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

The resulting animation provides the appearance of advancing the page background through the colors of the rainbow.

For more examples of creating complex animations, including a Bezier curve animation, see Chapter 22 of [Creating Mobile Apps with Xamarin.Forms](https://developer.xamarin.com/r/xamarin-forms/book/).

## Create a custom animation extension method

The extension methods in the [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) class animate a property from its current value to a specified value. This makes it difficult to create, for example, a `ColorTo` animation method that can be used to animate a color from one value to another, because:

- The only [`Color`](xref:Xamarin.Forms.Color) property defined by the [`VisualElement`](xref:Xamarin.Forms.VisualElement) class is [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor), which isn't always the desired `Color` property to animate.
- Often the current value of a [`Color`](xref:Xamarin.Forms.Color) property is [`Color.Default`](xref:Xamarin.Forms.Color.Default), which isn't a real color, and which can't be used in interpolation calculations.

The solution to this problem is to not have the `ColorTo` method target a particular [`Color`](xref:Xamarin.Forms.Color) property. Instead, it can be written with a callback method that passes the interpolated `Color` value back to the caller. In addition, the method will take start and end `Color` arguments.

The `ColorTo` method can be implemented as an extension method that uses the [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) method in the [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) class to provide its functionality. This is because the `Animate` method can be used to target properties that aren't of type `double`, as demonstrated in the following code example:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

The [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) method requires a *transform* argument, which is a callback method. The input to this callback is always a `double` ranging from 0 to 1. Therefore, the `ColorTo` method defines its own transform `Func` that accepts a `double` ranging from 0 to 1, and that returns a [`Color`](xref:Xamarin.Forms.Color) value corresponding to that value. The `Color` value is calculated by interpolating the [`R`](xref:Xamarin.Forms.Color.R), [`G`](xref:Xamarin.Forms.Color.G), [`B`](xref:Xamarin.Forms.Color.B), and [`A`](xref:Xamarin.Forms.Color.A) values of the two supplied `Color` arguments. The `Color` value is then passed to the callback method for application to a particular property.

This approach allows the `ColorTo` method to animate any [`Color`](xref:Xamarin.Forms.Color) property, as demonstrated in the following code example:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

In this code example, the `ColorTo` method animates the [`TextColor`](xref:Xamarin.Forms.Label.TextColor) and [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) properties of a [`Label`](xref:Xamarin.Forms.Label), the `BackgroundColor` property of a page, and the [`Color`](xref:Xamarin.Forms.BoxView.Color) property of a [`BoxView`](xref:Xamarin.Forms.BoxView).

## Related links

- [Custom Animations (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [Animation API](xref:Xamarin.Forms.Animation)
- [AnimationExtensions API](xref:Xamarin.Forms.AnimationExtensions)