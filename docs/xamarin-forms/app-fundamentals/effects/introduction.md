---
title: "Introduction to Effects"
description: "Effects allow the native controls on each platform to be customized, and are typically used for small styling changes. This article provides an introduction to effects, outlines the boundary between effects and custom renderers, and describes the PlatformEffect class."
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Introduction to Effects

_Effects allow the native controls on each platform to be customized, and are typically used for small styling changes. This article provides an introduction to effects, outlines the boundary between effects and custom renderers, and describes the PlatformEffect class._

Xamarin.Forms [Pages, Layouts and Controls](~/xamarin-forms/user-interface/controls/index.md) presents a common API to describe cross-platform mobile user interfaces. Each page, layout, and control is rendered differently on each platform using a `Renderer` class that in turn creates a native control (corresponding to the Xamarin.Forms representation), arranges it on the screen, and adds the behavior specified in the shared code.

Developers can implement their own custom `Renderer` classes to customize the appearance and/or behavior of a control. However, implementing a custom renderer class to perform a simple control customization is often a heavy-weight response. Effects simplify this process, allowing the native controls on each platform to be more easily customized.

Effects are created in platform-specific projects by subclassing the `PlatformEffect` control, and then the effects are consumed by attaching them to an appropriate control in a Xamarin.Forms .NET Standard library or Shared Library project.

## Why Use an Effect over a Custom Renderer?

Effects simplify the customization of a control, are reusable, and can be parameterized to further increase reuse.

Anything that can be achieved with an effect can also be achieved with a custom renderer. However, custom renderers offer more flexibility and customization than effects. The following guidelines list the circumstances in which to choose an effect over a custom renderer:

- An effect is recommended when changing the properties of a platform-specific control will achieve the desired result.
- A custom renderer is required when there's a need to override methods of a platform-specific control.
- A custom renderer is required when there's a need to replace the platform-specific control that implements a Xamarin.Forms control.

## Subclassing the PlatformEffect Class

The following table lists the namespace for the `PlatformEffect` class on each platform, and the types of its properties:

|Platform|Namespace|Container|Control|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|View|
|Universal Windows Platform (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Each platform-specific `PlatformEffect` class exposes the following properties:

- `Container` – references the platform-specific control being used to implement the layout.
- `Control` – references the platform-specific control being used to implement the Xamarin.Forms control.
- `Element` – references the Xamarin.Forms control that's being rendered.

Effects do not have type information about the container, control, or element they are attached to because they can be attached to any element. Therefore, when an effect is attached to an element that it doesn't support it should degrade gracefully or throw an exception. However, the `Container`, `Control`, and `Element` properties can be cast to their implementing type. For more information about these types see [Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Each platform-specific `PlatformEffect` class exposes the following methods, which must be overridden to implement an effect:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – called when an effect is attached to a Xamarin.Forms control. An overridden version of this method, in each platform-specific effect class, is the place to perform customization of the control, along with exception handling in case the effect cannot be applied to the specified Xamarin.Forms control.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – called when an effect is detached from a Xamarin.Forms control. An overridden version of this method, in each platform-specific effect class, is the place to perform any effect cleanup such as de-registering an event handler.

In addition, the `PlatformEffect` exposes the [`OnElementPropertyChanged`](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) method, which can also be overridden. This method is called when a property of the element has changed. An overridden version of this method, in each platform-specific effect class, is the place to respond to bindable property changes on the Xamarin.Forms control. A check for the property that's changed should always be made, as this override can be called many times.

## Related Links

- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
