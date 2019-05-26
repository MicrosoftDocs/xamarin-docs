---
title: "Bindable Properties"
description: "This article provides an introduction to bindable properties, and demonstrates how to create and consume them."
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
---

# Bindable Properties

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)

_In Xamarin.Forms, the functionality of common language runtime (CLR) properties is extended by bindable properties. A bindable property is a special type of property, where the property's value is tracked by the Xamarin.Forms property system. This article provides an introduction to bindable properties, and demonstrates how to create and consume them._

## Overview

Bindable properties extend CLR property functionality by backing a property with a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) type, instead of backing a property with a field. The purpose of bindable properties is to provide a property system that supports data binding, styles, templates, and values set through parent-child relationships. In addition, bindable properties can provide default values, validation of property values, and callbacks that monitor property changes.

Properties should be implemented as bindable properties to support one or more of the following features:

- Acting as a valid *target* property for data binding.
- Setting the property through a [style](~/xamarin-forms/user-interface/styles/index.md).
- Providing a default property value that's different from the default for the type of the property.
- Validating the value of the property.
- Monitoring property changes.

Examples of Xamarin.Forms bindable properties include [`Label.Text`](xref:Xamarin.Forms.Label.Text), [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius), and [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation). Each bindable property has a corresponding `public static readonly` property of type [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) that is exposed on the same class and that is the identifier of the bindable property. For example, the corresponding bindable property identifier for the `Label.Text` property is [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty).

<a name="consuming-bindable-property" />

## Creating and Consuming a Bindable Property

The process for creating a bindable property is as follows:

1. Create a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance with one of the [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) method overloads.
1. Define property accessors for the [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance.

Note that all [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instances must be created on the UI thread. This means that only code that runs on the UI thread can get or set the value of a bindable property. However, `BindableProperty` instances can be accessed from other threads by marshaling to the UI thread with the [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) method.

### Creating a Property

To create a `BindableProperty` instance, the containing class must derive from the [`BindableObject`](xref:Xamarin.Forms.BindableObject) class. However, the `BindableObject` class is high in the class hierarchy, so the majority of classes used for user interface functionality support bindable properties.

A bindable property can be created by declaring a `public static readonly` property of type [`BindableProperty`](xref:Xamarin.Forms.BindableProperty). The bindable property should be set to the returned value of one of the [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) method overloads. The declaration should be within the body of [`BindableObject`](xref:Xamarin.Forms.BindableObject) derived class, but outside of any member definitions.

At a minimum, an identifier must be specified when creating a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), along with the following parameters:

- The name of the [`BindableProperty`](xref:Xamarin.Forms.BindableProperty).
- The type of the property.
- The type of the owning object.
- The default value for the property. This ensures that the property always returns a particular default value when it is unset, and it can be different from the default value for the type of the property. The default value will be restored when the [`ClearValue`](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) method is called on the bindable property.

The following code shows an example of a bindable property, with an identifier and values for the four required parameters:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

This creates a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance named `EventName`, of type `string`. The property is owned by the `EventToCommandBehavior` class, and has a default value of `null`. The naming convention for bindable properties is that the bindable property identifier must match the property name specified in the `Create` method, with "Property" appended to it. Therefore, in the example above, the bindable property identifier is `EventNameProperty`.

Optionally, when creating a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance, the following parameters can be specified:

- The binding mode. This is used to specify the direction in which property value changes will propagate. In the default binding mode, changes will propagate from the *source* to the *target*.
- A validation delegate that will be invoked when the property value is set. For more information, see [Validation Callbacks](#validation).
- A property changed delegate that will be invoked when the property value has changed. For more information, see [Detecting Property Changes](#propertychanges).
- A property changing delegate that will be invoked when the property value will change. This delegate has the same signature as the property changed delegate.
- A coerce value delegate that will be invoked when the property value has changed. For more information, see [Coerce Value Callbacks](#coerce).
- A `Func` that's used to initialize a default property value. For more information, see [Creating a Default Value with a Func](#defaultfunc).

### Creating Accessors

Property accessors are required to use property syntax to access a bindable property. The `Get` accessor should return the value that's contained in the corresponding bindable property. This can be achieved by calling the [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) method, passing in the bindable property identifier on which to get the value, and then casting the result to the required type. The `Set` accessor should set the value of the corresponding bindable property. This can be achieved by calling the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method, passing in the bindable property identifier on which to set the value, and the value to set.

The following code example shows accessors for the `EventName` bindable property:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### Consuming a Bindable Property

Once a bindable property has been created, it can be consumed from XAML or code. In XAML, this is achieved by declaring a namespace with a prefix, with the namespace declaration indicating the CLR namespace name, and optionally, an assembly name. For more information, see [XAML Namespaces](~/xamarin-forms/xaml/namespaces.md).

The following code example demonstrates a XAML namespace for a custom type that contains a bindable property, which is defined within the same assembly as the application code that's referencing the custom type:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

The namespace declaration is used when setting the `EventName` bindable property, as demonstrated in the following XAML code example:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

The equivalent C# code is shown in the following code example:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## Advanced Scenarios

When creating a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) instance, there are a number of optional parameters that can be set to enable advanced bindable property scenarios. This section explores these scenarios.

<a name="propertychanges" />

### Detecting Property Changes

A `static` property-changed callback method can be registered with a bindable property by specifying the `propertyChanged` parameter for the [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) method. The specified callback method will be invoked when the value of the bindable property changes.

The following code example shows how the `EventName` bindable property registers the `OnEventNameChanged` method as a property-changed callback method:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

In the property-changed callback method, the [`BindableObject`](xref:Xamarin.Forms.BindableObject) parameter is used to denote which instance of the owning class has reported a change, and the values of the two `object` parameters represent the old and new values of the bindable property.

<a name="validation" />

### Validation Callbacks

A `static` validation callback method can be registered with a bindable property by specifying the `validateValue` parameter for the [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) method. The specified callback method will be invoked when the value of the bindable property is set.

The following code example shows how the `Angle` bindable property registers the `IsValidValue` method as a validation callback method:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Validation callbacks are provided with a value, and should return `true` if the value is valid for the property, otherwise `false`. An exception will be raised if a validation callback returns `false`, which should be handled by the developer. A typical use of a validation callback method is constraining the values of integers or doubles when the bindable property is set. For example, the `IsValidValue` method checks that the property value is a `double` within the range 0 to 360.

<a name="coerce" />

### Coerce Value Callbacks

A `static` coerce value callback method can be registered with a bindable property by specifying the `coerceValue` parameter for the [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) method. The specified callback method will be invoked when the value of the bindable property changes.

Coerce value callbacks are used to force a reevaluation of a bindable property when the value of the property changes. For example, a coerce value callback can be used to ensure that the value of one bindable property is not greater than the value of another bindable property.

The following code example shows how the `Angle` bindable property registers the `CoerceAngle` method as a coerce value callback method:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

The `CoerceAngle` method checks the value of the `MaximumAngle` property, and if the `Angle` property value is greater than it, it coerces the value to the `MaximumAngle` property value.

<a name="defaultfunc" />

### Creating a Default Value with a Func

A `Func` can be used to initialize the default value of a bindable property, as demonstrated in the following code example:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

The `defaultValueCreator` parameter is set to a `Func` that invokes the [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) method to return a `double` that represents the named size for the font that is used on a [`Label`](xref:Xamarin.Forms.Label) on the native platform.

## Summary

This article provided an introduction to bindable properties, and demonstrated how to create and consume them. A bindable property is a special type of property, where the property's value is tracked by the Xamarin.Forms property system.


## Related Links

- [XAML Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Event To Command Behavior (sample)](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)
- [Validation Callback (sample)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce Value Callback (sample)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
