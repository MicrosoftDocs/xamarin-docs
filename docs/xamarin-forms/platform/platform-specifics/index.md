---
title: "Platform-Specifics"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume and create platform-specifics."
ms.service: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Platform-Specifics

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects._

The process for consuming a platform-specific through XAML, or through the fluent code API is as follows:

1. Add a `xmlns` declaration or `using` directive for the [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) namespace.
1. Add a `xmlns` declaration or `using` directive for the namespace that contains the platform-specific functionality:
    1. On iOS, this is the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace.
    1. On Android, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) namespace. For Android AppCompat, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) namespace.
    1. On the Universal Windows Platform, this is the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace.
1. Apply the platform-specific from XAML, or from code with the `On<T>` fluent API. The value of `T` can be the [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS), [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android), or [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) types from the [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) namespace.

> [!NOTE]
> Note that attempting to consume a platform-specific on a platform where it is unavailable will not result in an error. Instead, the code will execute without the platform-specific being applied.

Platform-specifics consumed through the `On<T>` fluent code API return [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) objects. This allows multiple platform-specifics to be invoked on the same object with method cascading.

For more information about the platform-specifics provided by Xamarin.Forms, see [iOS Platform-Specifics](~/xamarin-forms/platform/ios/index.md), [Android Platform-Specifics](~/xamarin-forms/platform/android/index.md), and [Windows Platform-Specifics](~/xamarin-forms/platform/windows/index.md).

## Creating platform-specifics

Vendors can create their own platform-specifics with Effects. An Effect provides the specific functionality, which is then exposed through a platform-specific. The result is an Effect that can be more easily consumed through XAML, and through a fluent code API.

The process for creating a platform-specific is as follows:

1. Implement the specific functionality as an Effect. For more information, see [Creating an Effect](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Create a platform-specific class that will expose the Effect. For more information, see [Creating a Platform-Specific Class](#creating-a-platform-specific-class).
1. In the platform-specific class, implement an attached property to allow the platform-specific to be consumed through XAML. For more information, see [Adding an Attached Property](#adding-an-attached-property).
1. In the platform-specific class, implement extension methods to allow the platform-specific to be consumed through a fluent code API. For more information, see [Adding Extension Methods](#adding-extension-methods).
1. Modify the Effect implementation so that the Effect is only applied if the platform-specific has been invoked on the same platform as the Effect. For more information, see [Creating the Effect](#creating-the-effect).

The result of exposing an Effect as a platform-specific is that the Effect can be more easily consumed through XAML and through a fluent code API.

> [!NOTE]
> It's envisaged that vendors will use this technique to create their own platform-specifics, for ease of consumption by users. While users may choose to create their own platform-specifics, it should be noted that it requires more code than creating and consuming an Effect.

The [sample application](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) demonstrates a `Shadow` platform-specific that adds a shadow to the text displayed by a [`Label`](xref:Xamarin.Forms.Label) control:

![Shadow Platform-Specific](images/screenshots.png)

The [sample application](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) implements the `Shadow` platform-specific on each platform, for ease of understanding. However, aside from each platform-specific Effect implementation, the implementation of the Shadow class is largely identical for each platform. Therefore, this guide focusses on the implementation of the Shadow class and associated Effect on a single platform.

For more information about Effects, see [Customizing Controls with Effects](~/xamarin-forms/app-fundamentals/effects/index.md).

### Creating a platform-specific class

A platform-specific is created as a `public static` class:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

The following sections discuss the implementation of the `Shadow` platform-specific and associated Effect.

#### Adding an attached property

An attached property must be added to the `Shadow` platform-specific to allow consumption through XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

The `IsShadowed` attached property is used to add the `MyCompany.LabelShadowEffect` Effect to, and remove it from, the control that the `Shadow` class is attached to. This attached property registers the `OnIsShadowedPropertyChanged` method that will be executed when the value of the property changes. In turn, this method calls the `AttachEffect` or `DetachEffect` method to add or remove the effect based on the value of the `IsShadowed` attached property. The Effect is added to or removed from the control by modifying the control's [`Effects`](xref:Xamarin.Forms.Element.Effects) collection.

> [!NOTE]
> Note that the Effect is resolved by specifying a value that's a concatenation of the resolution group name and unique identifier that's specified on the Effect implementation. For more information, see [Creating an Effect](~/xamarin-forms/app-fundamentals/effects/creating.md).

For more information about attached properties, see [Attached Properties](~/xamarin-forms/xaml/attached-properties.md).

#### Adding Extension Methods

Extension methods must be added to the `Shadow` platform-specific to allow consumption through a fluent code API:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

The `IsShadowed` and `SetIsShadowed` extension methods invoke the get and set accessors for the `IsShadowed` attached property, respectively. Each extension method operates on the `IPlatformElementConfiguration<iOS, FormsElement>` type, which specifies that the platform-specific can be invoked on [`Label`](xref:Xamarin.Forms.Label) instances from iOS.

#### Creating the effect

The `Shadow` platform-specific adds the `MyCompany.LabelShadowEffect` to a [`Label`](xref:Xamarin.Forms.Label), and removes it. The following code example shows the `LabelShadowEffect` implementation for the iOS project:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
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

The `UpdateShadow` method sets `Control.Layer` properties to create the shadow, provided that the `IsShadowed` attached property is set to `true`, and provided that the `Shadow` platform-specific has been invoked on the same platform that the Effect is implemented for. This check is performed with the `OnThisPlatform` method.

If the `Shadow.IsShadowed` attached property value changes at runtime, the Effect needs to respond by removing the shadow. Therefore, an overridden version of the `OnElementPropertyChanged` method is used to respond to the bindable property change by calling the `UpdateShadow` method.

For more information about creating an effect, see [Creating an Effect](~/xamarin-forms/app-fundamentals/effects/creating.md) and [Passing Effect Parameters as Attached Properties](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### Consuming the platform-specific

The `Shadow` platform-specific is consumed in XAML by setting the `Shadow.IsShadowed` attached property to a `boolean` value:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [ShadowPlatformSpecific (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [iOS Platform-Specifics](~/xamarin-forms/platform/ios/index.md)
- [Android Platform-Specifics](~/xamarin-forms/platform/android/index.md)
- [Windows Platform-Specifics](~/xamarin-forms/platform/windows/index.md)
- [Customizing Controls with Effects](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Attached Properties](~/xamarin-forms/xaml/attached-properties.md)
- [PlatformConfiguration API](xref:Xamarin.Forms.PlatformConfiguration)