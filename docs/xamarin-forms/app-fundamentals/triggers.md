---
title: "Xamarin.Forms Triggers"
description: "This article explains how to use Xamarin.Forms triggers to respond to user interface changes with XAML. Triggers allow you to express actions declaratively in XAML that change the appearance of controls based on events or property changes."
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
---

# Xamarin.Forms Triggers

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/WorkingWithTriggers)

Triggers allow
    you to express actions declaratively in XAML that
    change the appearance of controls based on events
    or property changes.

You can assign a trigger directly to a control, or add it to
    a page-level or app-level resource dictionary to be applied
    to multiple controls.

There are four types of trigger:

* [Property Trigger](#property) - occurs when a property on a control
    is set to a particular value.

* [Data Trigger](#data) - uses data binding to trigger based on
    the properties of another control.

* [Event Trigger](#event) - occurs when an event occurs on the control.

* [Multi Trigger](#multi) - allows multiple trigger conditions to be
    set before an action occurs.

<a name="property" />

## Property Triggers

A simple trigger can be expressed purely in XAML, adding
    a `Trigger` element to a control's triggers collection.
    This example shows a trigger that changes an `Entry`
    background color when it receives focus:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

The important parts of the trigger's declaration are:

* **TargetType** - the control type that the trigger applies to.

* **Property** - the property on the control that is monitored.

* **Value** - the value, when it occurs for the monitored property,
    that causes the trigger to activate.

* **Setter** - a collection of `Setter` elements can be added
    and when the trigger condition is met. You must specify
    the `Property` and `Value` to set.

* **EnterActions and ExitActions** (not shown) - are written in
    code and can be used in
    addition to (or instead of) `Setter` elements. They
    are [described below](#enterexit).

### Applying a Trigger using a Style

Triggers can also be added to a `Style` declaration
    on a control, in a page, or an application `ResourceDictionary`. This
    example declares an implicit style (ie. no `Key` is
    set) which means it will apply to all `Entry` controls
    on the page.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## Data Triggers

Data triggers use data binding to monitor another control
    to cause the `Setter`s to get called. Instead of
    the `Property` attribute in a property trigger, set the
    `Binding` attribute to monitor for the
    specified value.

The example below uses the data binding syntax
    `{Binding Source={x:Reference entry}, Path=Text.Length}`
    which is how we refer to another control's properties. When the
    length of the `entry` is zero, the trigger is activated. In this
    sample the trigger disables the button when the input is empty.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

Tip: when evaluating `Path=Text.Length` always provide a
    default value for the target property (eg. `Text=""`)
    because otherwise it will be `null` and the trigger
    won't work like you expect.

In addition to specifying `Setter`s you can also provide
    [`EnterActions` and `ExitActions`](#enterexit).

<a name="event" />

## Event Triggers

The `EventTrigger` element
    requires only an `Event` property, such as `"Clicked"`
    in the example below.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Notice that there are no `Setter` elements but rather
    a reference to a class defined by `local:NumericValidationTriggerAction`
    which requires the `xmlns:local` to be declared in
    the page's XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

The class itself implements `TriggerAction` which means it should
    provide an override for the `Invoke` method that is called
    whenever the trigger event occurs.

A trigger action implementation should:

* Implement the generic `TriggerAction<T>` class, with the generic
    parameter corresponding with the type of control the trigger
    will be applied to. You can use superclasses such as `VisualElement`
    to write trigger actions that work with a variety of controls,
    or specify a control type like `Entry`.

* Override the `Invoke` method - this is called whenever the trigger
    criteria are met.

* Optionally expose properties that can be set in the XAML
    when the trigger is declared (such as `Anchor`, `Scale`, and
    `Length` in this example).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

The properties exposed by the trigger action can be set
    in the XAML declaration as follows:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Be careful when sharing triggers in a `ResourceDictionary`,
    one instance will be shared among controls so any state
    that is configured once will apply to them all.

Note that event triggers do not support `EnterActions`
    and `ExitActions`    [described below](#enterexit).

<a name="multi" />

## Multi Triggers

A `MultiTrigger` looks similar to a `Trigger` or `DataTrigger`
    except there can be more than one condition. All the conditions
    must be true before the `Setter`s are triggered.

Here's an example of a trigger for a button that binds to
    two different inputs (`email` and `phone`):

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

The `Conditions` collection could also contain
    `PropertyCondition` elements like this:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### Building a "require all" multi trigger

The multi trigger only updates its control when all conditions
    are true. Testing for "all field lengths are zero" (such as
    a login page where all inputs must be complete) is tricky
    because you want a condition "where Text.Length > 0" but
    this can't be expressed in XAML.

This can be done with an `IValueConverter`. The converter
    code below transforms the `Text.Length` binding into a
    `bool` that indicates whether a field is empty or not:

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

To use this converter in a multi trigger, first add it
    to the page's resource dictionary (along with a custom
    `xmlns:local` namespace definition):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

The XAML is shown below. Note the following differences
    from the first multi trigger example:

* The button has `IsEnabled="false"` set by default.
* The multi trigger conditions use the converter to
    turn the `Text.Length` value into a `boolean`.
* When all the conditions are `true`, the setter
    makes the button's `IsEnabled` property `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

These screenshots show the difference between the two multi
    trigger examples above. In the top part of the screens, text input in
    just one `Entry` is enough to enable the **Save** button.
    In the bottom part of the screens, the **Login** button
    remains inactive until both fields contain data.

![](triggers-images/multi-requireall.png "MultiTrigger Examples")

<a name="enterexit" />

## EnterActions and ExitActions

Another way to implement changes when a trigger occurs
    is by adding `EnterActions` and `ExitActions` collections
    and specifying `TriggerAction<T>` implementations.

You can provide *both* `EnterActions` and `ExitActions` as
    well as `Setter`s in a trigger, but be aware that the `Setter`s
    are called immediately (they do not wait for the `EnterAction`
    or `ExitAction` to complete). Alternatively you can perform
    everything in the code and not use `Setter`s at all.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

As always, when a class is referenced in XAML you should
    declare a namespace such as `xmlns:local` as shown here:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

The `FadeTriggerAction` code is shown below:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Note: `EnterActions` and `ExitActions` are ignored on
    **Event Triggers**.



## Related Links

- [Triggers Sample](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API Documentation](xref:Xamarin.Forms.TriggerAction`1)
