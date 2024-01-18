---
title: "Reusable EffectBehavior"
description: "Behaviors are a useful approach for adding an effect to a control, removing boiler-plate effect handling code from code-behind files. This article demonstrates creating and consuming a Xamarin.Forms behavior to add an effect to a control."
ms.service: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Reusable EffectBehavior

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)

_Behaviors are a useful approach for adding an effect to a control, removing boiler-plate effect handling code from code-behind files. This article demonstrates creating and consuming a Xamarin.Forms behavior to add an effect to a control._

## Overview

The `EffectBehavior` class is a reusable Xamarin.Forms custom behavior that adds an [`Effect`](xref:Xamarin.Forms.Effect) instance to a control when the behavior is attached to the control, and removes the `Effect` instance when the behavior is detached from the control.

The following behavior properties must be set to use the behavior:

- **Group** – the value of the [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribute for the effect class.
- **Name** – the value of the [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attribute for the effect class.

For more information about effects, see [Effects](~/xamarin-forms/app-fundamentals/effects/index.md).

> [!NOTE]
> The `EffectBehavior` is a custom class that can be located in the [Effect Behavior sample](/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior), and is not part of Xamarin.Forms.

## Creating the Behavior

The `EffectBehavior` class derives from the [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) class, where `T` is a [`View`](xref:Xamarin.Forms.View). This means that the `EffectBehavior` class can be attached to any Xamarin.Forms control.

### Implementing Bindable Properties

The `EffectBehavior` class defines two [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instances, which are used to add an [`Effect`](xref:Xamarin.Forms.Effect) to a control when the behavior is attached to the control. These properties are shown in the following code example:

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

When the `EffectBehavior` is consumed, the `Group` property should be set to the value of the [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribute for the effect. In addition, the `Name` property should be set to the value of the [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attribute for the effect class.

### Implementing the Overrides

The `EffectBehavior` class overrides the [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) and [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) methods of the [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) class, as shown in the following code example:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

The [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method performs setup by calling the `AddEffect` method, passing in the attached control as a parameter. The [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) method performs cleanup by calling the `RemoveEffect` method, passing in the attached control as a parameter.

### Implementing the Behavior Functionality

The purpose of the behavior is to add the [`Effect`](xref:Xamarin.Forms.Effect) defined in the `Group` and `Name` properties to a control when the behavior is attached to the control, and remove the `Effect` when the behavior is detached from the control. The core behavior functionality is shown in the following code example:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

The `AddEffect` method is executed in response to the `EffectBehavior` being attached to a control, and it receives the attached control as a parameter. The method then adds the retrieved effect to the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection. The `RemoveEffect` method is executed in response to the `EffectBehavior` being detached from a control, and it receives the attached control as a parameter. The method then removes the effect from the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection.

The `GetEffect` method uses the [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) method to retrieve the [`Effect`](xref:Xamarin.Forms.Effect). The effect is located through a concatenation of the `Group` and `Name` property values. If a platform doesn't provide the effect, the `Effect.Resolve` method will return a non-`null` value.

## Consuming the Behavior

The `EffectBehavior` class can be attached to the [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) collection of a control, as demonstrated in the following XAML code example:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

The equivalent C# code is shown in the following code example:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

The `Group` and `Name` properties of the behavior are set to the values of the [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) and [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attributes for the effect class in each platform-specific project.

At runtime, when the behavior is attached to the [`Label`](xref:Xamarin.Forms.Label) control, the `Xamarin.LabelShadowEffect` will be added to the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection. This results in a shadow being added to the text displayed by the `Label` control, as shown in the following screenshots:

![Sample Application with EffectsBehavior](effect-behavior-images/screenshots.png)

The advantage of using this behavior to add and remove effects from controls is that boiler-plate effect-handling code can be removed from code-behind files.

## Summary

This article demonstrated using a behavior to add an effect to a control. The `EffectBehavior` class is a reusable Xamarin.Forms custom behavior that adds an [`Effect`](xref:Xamarin.Forms.Effect) instance to a control when the behavior is attached to the control, and removes the `Effect` instance when the behavior is detached from the control.

## Related Links

- [Effects](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Effect Behavior (sample)](/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)