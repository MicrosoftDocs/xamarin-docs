---
title: "Creating an Effect"
description: "Effects simplify the customization of a control. This article demonstrates how to create an effect that changes the background color of the Entry control when the control gains focus."
ms.service: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Creating an Effect

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/effects-focuseffect)

_Effects simplify the customization of a control. This article demonstrates how to create an effect that changes the background color of the Entry control when the control gains focus._

The process for creating an effect in each platform-specific project is as follows:

1. Create a subclass of the `PlatformEffect` class.
1. Override the `OnAttached` method and write logic to customize the control.
1. Override the `OnDetached` method and write logic to clean up the control customization, if required.
1. Add a [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribute to the effect class. This attribute sets a company wide namespace for effects, preventing collisions with other effects with the same name. Note that this attribute can only be applied once per project.
1. Add an [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attribute to the effect class. This attribute registers the effect with a unique ID that's used by Xamarin.Forms, along with the group name, to locate the effect prior to applying it to a control. The attribute takes two parameters – the type name of the effect, and a unique string that will be used to locate the effect prior to applying it to a control.

The effect can then be consumed by attaching it to the appropriate control.

> [!NOTE]
> It's optional to provide an effect in each platform project. Attempting to use an effect when one isn't registered will return a non-null value that does nothing.

The sample application demonstrates a `FocusEffect` that changes the background color of a control when it gains focus. The following diagram illustrates the responsibilities of each project in the sample application, along with the relationships between them:

![Focus Effect Project Responsibilities](creating-images/focus-effect.png)

An [`Entry`](xref:Xamarin.Forms.Entry) control on the `HomePage` is customized by the `FocusEffect` class in each platform-specific project. Each `FocusEffect` class derives from the `PlatformEffect` class for each platform. This results in the `Entry` control being rendered with a platform-specific background color, which changes when the control gains focus, as shown in the following screenshots:

![Focus Effect on each Platform, control focused](creating-images/screenshots-1.png)
![Focus Effect on each Platform, control unfocused](creating-images/screenshots-2.png)

## Creating the Effect on Each Platform

The following sections discuss the platform-specific implementation of the `FocusEffect` class.

## iOS Project

The following code example shows the `FocusEffect` implementation for the iOS project:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(EffectsDemo.iOS.FocusEffect), nameof(EffectsDemo.iOS.FocusEffect))]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

The `OnAttached` method sets the `BackgroundColor` property of the control to light purple with the `UIColor.FromRGB` method, and also stores this color in a field. This functionality is wrapped in a `try`/`catch` block in case the control the effect is attached to does not have a `BackgroundColor` property. No implementation is provided by the `OnDetached` method because no cleanup is necessary.

The `OnElementPropertyChanged` override responds to bindable property changes on the Xamarin.Forms control. When the [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) property changes, the `BackgroundColor` property of the control is changed to white if the control has focus, otherwise it's changed to light purple. This functionality is wrapped in a `try`/`catch` block in case the control the effect is attached to does not have a `BackgroundColor` property.

## Android Project

The following code example shows the `FocusEffect` implementation for the Android project:

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.Droid.FocusEffect), nameof(EffectsDemo.Droid.FocusEffect))]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color originalBackgroundColor = new Android.Graphics.Color(0, 0, 0, 0);
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached()
        {
            try
            {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor(backgroundColor);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);
            try
            {
                if (args.PropertyName == "IsFocused")
                {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor)
                    {
                        Control.SetBackgroundColor(originalBackgroundColor);
                    }
                    else
                    {
                        Control.SetBackgroundColor(backgroundColor);
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

The `OnAttached` method calls the `SetBackgroundColor` method to set the background color of the control to light green, and also stores this color in a field. This functionality is wrapped in a `try`/`catch` block in case the control the effect is attached to does not have a `SetBackgroundColor` property. No implementation is provided by the `OnDetached` method because no cleanup is necessary.

The `OnElementPropertyChanged` override responds to bindable property changes on the Xamarin.Forms control. When the [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) property changes, the background color of the control is changed to white if the control has focus, otherwise it's changed to light green. This functionality is wrapped in a `try`/`catch` block in case the control the effect is attached to does not have a `BackgroundColor` property.

## Universal Windows Platform Projects

The following code example shows the `FocusEffect` implementation for Universal Windows Platform (UWP) projects:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.UWP.FocusEffect), nameof(EffectsDemo.UWP.FocusEffect))]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

The `OnAttached` method sets the `Background` property of the control to cyan, and sets the `BackgroundFocusBrush` property to white. This functionality is wrapped in a `try`/`catch` block in case the control the effect is attached to lacks these properties. No implementation is provided by the `OnDetached` method because no cleanup is necessary.

## Consuming the Effect

The process for consuming an effect from a Xamarin.Forms .NET Standard library or Shared Library project is as follows:

1. Declare a control that will be customized by the effect.
1. Attach the effect to the control by adding it to the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection.

> [!NOTE]
> An effect instance can only be attached to a single control. Therefore, an effect must be resolved twice to use it on two controls.

## Consuming the Effect in XAML

The following XAML code example shows an [`Entry`](xref:Xamarin.Forms.Entry) control to which the `FocusEffect` is attached:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

The `FocusEffect` class in the .NET Standard library supports effect consumption in XAML, and is shown in the following code example:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ($"MyCompany.{nameof(FocusEffect)}")
    {
    }
}
```

The `FocusEffect` class subclasses the [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) class, which represents a platform-independent effect that wraps an inner effect that is usually platform-specific. The `FocusEffect` class calls the base class constructor, passing in a parameter consisting of a concatenation of the resolution group name (specified using the [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribute on the effect class), and the unique ID that was specified using the [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attribute on the effect class. Therefore, when the [`Entry`](xref:Xamarin.Forms.Entry) is initialized at runtime, a new instance of the `MyCompany.FocusEffect` is added to the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection.

Effects can also be attached to controls by using a behavior, or by using attached properties. For more information about attaching an effect to a control by using a behavior, see [Reusable EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/effect-behavior.md). For more information about attaching an effect to a control by using attached properties, see [Passing Parameters to an Effect](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## Consuming the Effect in C&num;

The equivalent [`Entry`](xref:Xamarin.Forms.Entry) in C# is shown in the following code example:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

The `FocusEffect` is attached to the `Entry` instance by adding the effect to the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection, as demonstrated in the following code example:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ($"MyCompany.{nameof(FocusEffect)}"));
  ...
}
```

The [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) returns an [`Effect`](xref:Xamarin.Forms.Effect) for the specified name, which is a concatenation of the resolution group name (specified using the [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribute on the effect class), and the unique ID that was specified using the [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) attribute on the effect class. If a platform doesn't provide the effect, the `Effect.Resolve` method will return a non-`null` value.

## Summary

This article demonstrated how to create an effect that changes the background color of the [`Entry`](xref:Xamarin.Forms.Entry) control when the control gains focus.

## Related Links

- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Background Color Effect (sample)](/samples/xamarin/xamarin-forms-samples/effects-backgroundcoloreffect)
- [Focus Effect (sample)](/samples/xamarin/xamarin-forms-samples/effects-focuseffect)