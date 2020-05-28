---
title: "Xamarin.Forms Effects"
description: "Effects allow the native controls on each platform to be customized without having to resort to a custom renderer implementation."
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Effects

_Xamarin.Forms user interfaces are rendered using the native controls of the target platform, allowing Xamarin.Forms applications to retain the appropriate look and feel for each platform. Effects allow the native controls on each platform to be customized without having to resort to a custom renderer implementation._

## [Introduction to Effects](introduction.md)

Effects allow the native controls on each platform to be customized, and are typically used for small styling changes. This article provides an introduction to effects, outlines the boundary between effects and custom renderers, and describes the `PlatformEffect` class.

## [Creating an Effect](creating.md)

Effects simplify the customization of a control. This article demonstrates how to create an effect that changes the background color of the [`Entry`](xref:Xamarin.Forms.Entry) control when the control gains focus.

## [Passing Parameters to an Effect](passing-parameters/index.md)

Creating an effect that's configured through parameters enables the effect to be reused. These articles demonstrate using properties to pass parameters to an effect, and changing a parameter at runtime.

## [Invoking Events from an Effect](touch-tracking.md)

Effects can invoke events. This article shows how to create an event that implements low-level multi-touch finger tracking and signals an application for touch presses, moves, and releases.

## [Reusable RoundEffect](reusable-roundeffect.md)

RoundEffect is a reusable effect that can be applied to any control deriving from VisualElement to render the control as a circle. This effect can be used to create circular images, circular buttons, or other circular controls.
