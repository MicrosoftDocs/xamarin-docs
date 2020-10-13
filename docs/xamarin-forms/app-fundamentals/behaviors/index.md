---
title: "Xamarin.Forms Behaviors"
description: "Behaviors lets you add functionality to user interface controls without having to subclass them. Behaviors are written in code and added to controls in XAML or code."
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Behaviors

_Behaviors lets you add functionality to user interface controls without having to subclass them. Behaviors are written in code and added to controls in XAML or code._

## [Introduction to Behaviors](introduction.md)

Behaviors enable you to implement code that you would normally have to write as code-behind, because it directly interacts with the API of the control in such a way that it can be concisely attached to the control. This article provides an introduction to behaviors.

## [Attached Behaviors](attached.md)

Attached behaviors are `static` classes with one or more attached properties. This article demonstrates how to create and consume attached behaviors.

## [Xamarin.Forms Behaviors](creating.md)

Xamarin.Forms behaviors are created by deriving from the [`Behavior`](xref:Xamarin.Forms.Behavior) or [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) class. This article demonstrates how to create and consume Xamarin.Forms behaviors.

## [Reusable EffectBehavior](effect-behavior.md)

Behaviors are a useful approach for adding an effect to a control, removing boiler-plate effect handling code from code-behind files. This article demonstrates creating and consuming a Xamarin.Forms behavior to add an effect to a control.
