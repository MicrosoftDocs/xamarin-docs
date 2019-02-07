---
title: "Bindable Layouts in Xamarin.Forms"
description: "Bindable layouts enable layout classes to generate their content by binding to a collection of items, with the option to set the appearance of each item with a DataTemplate."
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
---

# Bindable Layouts in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

Bindable layouts enable any layout class that derives from the [`Layout<T>`](xref:Xamarin.Forms.Layout`1) class to generate its content by binding to a collection of items, with the option to set the appearance of each item with a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Bindable layouts are provided by the `BindableLayout` class, which exposes the following attached properties:

- `ItemsSource` – specifies the collection of `IEnumerable` items to be displayed by the layout.
- `ItemTemplate` – specifies the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) to apply to each item in the collection of items displayed by the layout.
- `ItemTemplateSelector` – specifies the [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) that will be used to choose a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) for an item at runtime.

These properties can be attached to the [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`FlexLayout`](xref:Xamarin.Forms.FlexLayout), [`Grid`](xref:Xamarin.Forms.Grid), [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout), and [`StackLayout`](xref:Xamarin.Forms.StackLayout) classes, which all derive from the [`Layout<T>`](xref:Xamarin.Forms.Layout`1) class.

> [!NOTE]
> The `ItemTemplate` property takes precedence when both the `ItemTemplate` and `ItemTemplateSelector` properties are set.

The `Layout<T>` class exposes a [`Children`](xref:Xamarin.Forms.Layout`1.Children) collection, to which the child elements of a layout are added. When the `BinableLayout.ItemsSource` property is set to a collection of items and attached to a [`Layout<T>`](xref:Xamarin.Forms.Layout`1)-derived class, each item in the collection is added to the `Layout<T>.Children` collection for display by the layout. The `Layout<T>`-derived class will then update its child views when the underlying collection changes. For more information about the Xamarin.Forms layout cycle, see [Creating a Custom Layout](~/xamarin-forms/user-interface/layouts/custom.md).

> [!IMPORTANT]
> Bindable layouts should only be used when the collection of items to be displayed is small, and scrolling and selection isn't required. While scrolling can be provided by wrapping a bindable layout in a [`ScrollView`](xref:Xamarin.Forms.ScrollView), this is not recommended as bindable layouts lack UI virtualization. When scrolling is required, a scrollable view that includes UI virtualization, such as [`ListView`](xref:Xamarin.Forms.ListView) or `CollectionView`, should be used. Failure to observe this recommendation can lead to performance issues.

## Populating a bindable layout with data

A bindable layout is populated with data by setting its `ItemsSource` property to any collection that implements `IEnumerable`, and attaching it to a [`Layout<T>`](xref:Xamarin.Forms.Layout`1)-derived class:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

The equivalent C# code is:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

When the `BindableLayout.ItemsSource` attached property is set on a layout, but the `BindableLayout.ItemTemplate` attached property isn't set, every item in the `IEnumerable` collection will be displayed by a [`Label`](xref:Xamarin.Forms.Label) that's created by the `BindableLayout` class.

## Defining item appearance

The appearance of each item in the bindable layout can be defined by setting the `BindableLayout.ItemTemplate` attached property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

The equivalent C# code is:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

In this example, every item in the `TopFollowers` collection will be displayed by a `CircleImage` view defined in the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

![Bindable layout with a DataTemplate](bindable-layouts-images/top-followers.png "Bindable layout with a data template")

For more information about data templates, see [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## Choosing item appearance at runtime

The appearance of each item in the bindable layout can be chosen at runtime, based on the item value, by setting the `BindableLayout.ItemTemplateSelector` attached property to a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector):

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

The equivalent C# code is:

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

The [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) used in the sample application is shown in the following example:

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

The `TechItemTemplateSelector` class defines `DefaultTemplate` and `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) properties that are set to different data templates. The `OnSelectTemplate` method returns the `XamarinFormsTemplate`, which displays an item in dark red with a heart next to it, when the item is equal to "Xamarin.Forms". When the item isn't equal to "Xamarin.Forms", the `OnSelectTemplate` method returns the `DefaultTemplate`, which displays an item using the default color of a [`Label`](xref:Xamarin.Forms.Label):

![Bindable layout with a DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Bindable layout with a data template selector")

For more information about data template selectors, see [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## Related links

- [Bindable Layout Demo (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [Creating a Custom Layout](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
