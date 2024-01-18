---
title: "Xamarin.Forms Binding Fallbacks"
description: "This article explains how to make bindings more robust by defining fallback values that will be used if binding fails."
ms.service: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Binding Fallbacks

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Sometimes data bindings fail, because the binding source can't be resolved, or because the binding succeeds but returns a `null` value. While these scenarios can be handled with value converters, or other additional code, data bindings can be made more robust by defining fallback values to use if the binding process fails. This can be accomplished by defining the [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) and [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) properties in a binding expression. Because these properties reside in the [`BindingBase`](xref:Xamarin.Forms.BindingBase) class, they can be used with bindings, multi-bindings, compiled bindings, and with the `Binding` markup extension.

> [!NOTE]
> Use of the [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) and [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) properties in a binding expression is optional.

## Defining a fallback value

The [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) property allows a fallback value to be defined that will be used when the binding *source* can't be resolved. A common scenario for setting this property is when binding to source properties that might not exist on all objects in a bound collection of heterogeneous types.

The **MonkeyDetail** page illustrates setting the [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) property:

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

The binding on the [`Label`](xref:Xamarin.Forms.Label) defines a [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) value that will be set on the target if the binding source can't be resolved. Therefore, the value defined by the `FallbackValue` property will be displayed if the `Population` property doesn't exist on the bound object. Notice that here the `FallbackValue` property value is delimited by single-quote (apostrophe) characters.

Rather than defining [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) property values inline, it's recommended to define them as resources in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The advantage of this approach is that such values are defined once in a single location, and are more easily localizable. The resources can then be retrieved using the `StaticResource` markup extension:

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> It's not possible to set the `FallbackValue` property with a binding expression.

Here's the program running:

![FallbackValue Binding](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue Binding")

When the `FallbackValue` property isn't set in a binding expression and the binding path or part of the path isn't resolved, [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) is set on the target. However, when the `FallbackValue` property is set and the binding path or part of the path isn't resolved, the value of the `FallbackValue` value property is set on the target. Therefore, on the **MonkeyDetail** page the [`Label`](xref:Xamarin.Forms.Label) displays "Population size unknown" because the bound object lacks a `Population` property.

> [!IMPORTANT]
> A defined value converter is not executed in a binding expression when the [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) property is set.

## Defining a null replacement value

The [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) property allows a replacement value to be defined that will be used when the binding *source* is resolved, but the value is `null`. A common scenario for setting this property is when binding to source properties that might be `null` in a bound collection.

The **Monkeys** page illustrates setting the [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) property:

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

The bindings on the [`Image`](xref:Xamarin.Forms.Image) and [`Label`](xref:Xamarin.Forms.Label) both define [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) values that will be applied if the binding path returns `null`. Therefore, the values defined by the `TargetNullValue` properties will be displayed for any objects in the collection where the `ImageUrl` and `Location` properties are not defined. Notice that here the `TargetNullValue` property values are delimited by single-quote (apostrophe) characters.

Rather than defining [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) property values inline, it's recommended to define them as resources in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The advantage of this approach is that such values are defined once in a single location, and are more easily localizable. The resources can then be retrieved using the `StaticResource` markup extension:

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> It's not possible to set the `TargetNullValue` property with a binding expression.

Here's the program running:

[![TargetNullValue Binding](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue Binding")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue Binding")

When the `TargetNullValue` property isn't set in a binding expression, a source value of `null` will be converted if a value converter is defined, formatted if a `StringFormat` is defined, and the result is then set on the target. However, when the `TargetNullValue` property is set, a source value of `null` will be converted if a value converter is defined, and if it's still `null` after the conversion, the value of the `TargetNullValue` property is set on the target.

> [!IMPORTANT]
> String formatting is not applied in a binding expression when the `TargetNullValue` property is set.

## Related Links

- [Data Binding Demos (sample)](/samples/xamarin/xamarin-forms-samples/databindingdemos)