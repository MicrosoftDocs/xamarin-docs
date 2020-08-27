---
title: "Introduction to Behaviors"
description: "Behaviors let you add functionality to user interface controls without having to subclass them. Instead, the functionality is implemented in a behavior class and attached to the control as if it was part of the control itself. This article provides an introduction to behaviors."
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Introduction to Behaviors

_Behaviors let you add functionality to user interface controls without having to subclass them. Instead, the functionality is implemented in a behavior class and attached to the control as if it was part of the control itself. This article provides an introduction to behaviors._

Behaviors enable you to implement code that you would normally have to write as code-behind, because it directly interacts with the API of the control in such a way that it can be concisely attached to the control and packaged for reuse across more than one application. They can be used to provide a full range of functionality to controls, such as:

- Adding an email validator to an [`Entry`](xref:Xamarin.Forms.Entry).
- Creating a rating control using a tap gesture recognizer.
- Controlling an animation.
- Adding an effect to a control.

Behaviors also enable more advanced scenarios. In the context of *commanding*, behaviors are a useful approach for connecting a control to a command. In addition, they can be used to associate commands with controls that were not designed to interact with commands. For example, they can be used to invoke a command in response to an event firing.

Xamarin.Forms supports two different styles of behaviors:

- **Xamarin.Forms behaviors** – classes that derive from the [`Behavior`](xref:Xamarin.Forms.Behavior) or [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) class, where `T` is the type of the control to which the behavior should apply. For more information about Xamarin.Forms behaviors, see [Xamarin.Forms Behaviors](~/xamarin-forms/app-fundamentals/behaviors/creating.md).
- **Attached behaviors** – `static` classes with one or more attached properties. For more information about attached behaviors, see [Attached Behaviors](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

This guide focuses on Xamarin.Forms behaviors because they are the preferred approach to behavior construction.

## Related Links

- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
