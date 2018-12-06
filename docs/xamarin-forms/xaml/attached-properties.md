---
title: "Attached Properties"
description: "This article provides an introduction to attached properties, and demonstrates how to create and consume them."
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
---

# Attached Properties

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)

_An attached property is a special type of bindable property, defined in one class but attached to other objects, and recognizable in XAML as an attribute that contains a class and a property name separated by a period. This article provides an introduction to attached properties, and demonstrates how to create and consume them._

## Overview

Attached properties enable an object to assign a value for a property that its own class doesn't define. For example, child elements can use attached properties to inform their parent element of how they are to be presented in the user interface. The [`Grid`](xref:Xamarin.Forms.Grid) control allows the row and column of a child to be specified by setting the `Grid.Row` and `Grid.Column` attached properties. `Grid.Row` and `Grid.Column` are attached properties because they are set on elements that are children of a `Grid`, rather than on the `Grid` itself.

Bindable properties should be implemented as attached properties in the following scenarios:

- When there's a need to have a property setting mechanism available for classes other than the defining class.
- When the class represents a service that needs to be easily integrated with other classes.

For more information about bindable properties, see [Bindable Properties](~/xamarin-forms/xaml/bindable-properties.md).

## Creating and Consuming an Attached Property

The process for creating an attached property is as follows:

1. Create a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance with one of the [`CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached*) method overloads.
1. Provide `static` `Get`*PropertyName* and `Set`*PropertyName* methods as accessors for the attached property.

### Creating a Property

When creating an attached property for use on other types, the class where the property is created does not have to derive from [`BindableObject`](xref:Xamarin.Forms.BindableObject). However, the *target* property for accessors should be of, or derive from, [`BindableObject`](xref:Xamarin.Forms.BindableObject).

An attached property can be created by declaring a `public static readonly` property of type [`BindableProperty`](xref:Xamarin.Forms.BindableProperty). The bindable property should be set to the returned value of one of the [`BindableProperty.CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) method overloads. The declaration should be within the body of the owning class, but outside of any member definitions.

The following code shows an example of an attached property:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

This creates an attached property named `HasShadow`, of type `bool`. The property is owned by the `ShadowEffect` class, and has a default value of `false`. The naming convention for attached properties is that the attached property identifier must match the property name specified in the `CreateAttached` method, with "Property" appended to it. Therefore, in the example above, the attached property identifier is `HasShadowProperty`.

For more information about creating bindable properties, including parameters that can be specified during creation, see [Creating and Consuming a Bindable Property](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### Creating Accessors

Static `Get`*PropertyName* and `Set`*PropertyName* methods are required as accessors for the attached property, otherwise the property system will be unable to use the attached property. The `Get`*PropertyName* accessor should conform to the following signature:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

The `Get`*PropertyName* accessor should return the value that's contained in the corresponding `BindableProperty` field for the attached property. This can be achieved by calling the [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) method, passing in the bindable property identifier on which to get the value, and then casting the resulting value to the required type.

The `Set`*PropertyName* accessor should conform to the following signature:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

The `Set`*PropertyName* accessor should set the value of the corresponding `BindableProperty` field for the attached property. This can be achieved by calling the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method, passing in the bindable property identifier on which to set the value, and the value to set.

For both accessors, the *target* object should be of, or derive from, [`BindableObject`](xref:Xamarin.Forms.BindableObject).

The following code example shows accessors for the `HasShadow` attached property:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### Consuming an Attached Property

Once an attached property has been created, it can be consumed from XAML or code. In XAML, this is achieved by declaring a namespace with a prefix, with the namespace declaration indicating the Common Language Runtime (CLR) namespace name, and optionally an assembly name. For more information, see [XAML Namespaces](~/xamarin-forms/xaml/namespaces.md).

The following code example demonstrates a XAML namespace for a custom type that contains an attached property, which is defined within the same assembly as the application code that's referencing the custom type:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

The namespace declaration is then used when setting the attached property on a specific control, as demonstrated in the following XAML code example:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

The equivalent C# code is shown in the following code example:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### Consuming an Attached Property with a Style

Attached properties can also be added to a control by a style. The following XAML code example shows an *explicit* style that uses the `HasShadow` attached property, that can be applied to [`Label`](xref:Xamarin.Forms.Label) controls:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

The [`Style`](xref:Xamarin.Forms.Style) can be applied to a [`Label`](xref:Xamarin.Forms.Label) by setting its [`Style`](xref:Xamarin.Forms.VisualElement.Style) property to the `Style` instance using the `StaticResource` markup extension, as demonstrated in the following code example:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

For more information about styles, see [Styles](~/xamarin-forms/user-interface/styles/index.md).

## Advanced Scenarios

When creating an attached property, there are a number of optional parameters that can be set to enable advanced attached property scenarios. This includes detecting property changes, validating property values, and coercing property values. For more information, see [Advanced Scenarios](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## Summary

This article provided an introduction to attached properties, and demonstrated how to create and consume them. An attached property is a special type of bindable property, defined in one class but attached to other objects, and recognizable in XAML as attributes that contain a class and a property name separated by a period.


## Related Links

- [Bindable Properties](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Shadow Effect (sample)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
