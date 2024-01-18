---
title: "Bindable Layouts in Xamarin.Forms"
description: "Bindable layouts enable layout classes to generate their content by binding to a collection of items, with the option to set the appearance of each item with a DataTemplate."
ms.service: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/09/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Bindable Layouts in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Bindable layouts enable any layout class that derives from the [`Layout<T>`](xref:Xamarin.Forms.Layout`1) class to generate its content by binding to a collection of items, with the option to set the appearance of each item with a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Bindable layouts are provided by the `BindableLayout` class, which exposes the following attached properties:

- `ItemsSource` – specifies the collection of `IEnumerable` items to be displayed by the layout.
- `ItemTemplate` – specifies the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) to apply to each item in the collection of items displayed by the layout.
- `ItemTemplateSelector` – specifies the [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) that will be used to choose a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) for an item at runtime.

> [!NOTE]
> The `ItemTemplate` property takes precedence when both the `ItemTemplate` and `ItemTemplateSelector` properties are set.

In addition, the `BindableLayout` class exposes the following bindable properties:

- `EmptyView` – specifies the `string` or view that will be displayed when the `ItemsSource` property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The default value is `null`.
- `EmptyViewTemplate` – specifies the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) that will be displayed when the `ItemsSource` property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The default value is `null`.

> [!NOTE]
> The `EmptyViewTemplate` property takes precedence when both the `EmptyView` and `EmptyViewTemplate` properties are set.

All of these properties can be attached to the [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`FlexLayout`](xref:Xamarin.Forms.FlexLayout), [`Grid`](xref:Xamarin.Forms.Grid), [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout), and [`StackLayout`](xref:Xamarin.Forms.StackLayout) classes, which all derive from the [`Layout<T>`](xref:Xamarin.Forms.Layout`1) class.

The `Layout<T>` class exposes a [`Children`](xref:Xamarin.Forms.Layout`1.Children) collection, to which the child elements of a layout are added. When the `BindableLayout.ItemsSource` property is set to a collection of items and attached to a [`Layout<T>`](xref:Xamarin.Forms.Layout`1)-derived class, each item in the collection is added to the `Layout<T>.Children` collection for display by the layout. The `Layout<T>`-derived class will then update its child views when the underlying collection changes. For more information about the Xamarin.Forms layout cycle, see [Creating a Custom Layout](~/xamarin-forms/user-interface/layouts/custom.md).

Bindable layouts should only be used when the collection of items to be displayed is small, and scrolling and selection isn't required. While scrolling can be provided by wrapping a bindable layout in a [`ScrollView`](xref:Xamarin.Forms.ScrollView), this is not recommended as bindable layouts lack UI virtualization. When scrolling is required, a scrollable view that includes UI virtualization, such as [`ListView`](xref:Xamarin.Forms.ListView) or [`CollectionView`](xref:Xamarin.Forms.CollectionView), should be used. Failure to observe this recommendation can lead to performance issues.

> [!IMPORTANT]
> While it's technically possible to attach a bindable layout to any layout class that derives from the [`Layout<T>`](xref:Xamarin.Forms.Layout`1) class, it's not always practical to do so, particularly for the [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`Grid`](xref:Xamarin.Forms.Grid), and [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) classes. For example, consider the scenario of wanting to display a collection of data in a [`Grid`](xref:Xamarin.Forms.Grid) using a bindable layout, where each item in the collection is an object containing multiple properties. Each row in the `Grid` should display an object from the collection, with each column in the `Grid` displaying one of the object's properties. Because the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) for the bindable layout can only contain a single object, it's necessary for that object to be a layout class containing multiple views that each display one of the object's properties in a specific `Grid` column. While this scenario can be realised with bindable layouts, it results in a parent `Grid` containing a child `Grid` for each item in the bound collection, which is a highly inefficient and problematic use of the `Grid` layout.

## Populate a bindable layout with data

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

## Define item appearance

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

## Choose item appearance at runtime

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

## Display a string when data is unavailable

The `EmptyView` property can be set to a string, which will be displayed by a [`Label`](xref:Xamarin.Forms.Label) when the `ItemsSource` property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The following XAML shows an example of this scenario:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

The result is that when the data bound collection is `null`, the string set as the `EmptyView` property value is displayed:

[![Screenshot of a bindable layout string empty view, on iOS and Android](bindable-layouts-images/emptyview-string.png "Bindable layout string empty view")](bindable-layouts-images/emptyview-string-large.png#lightbox "Bindable layout string empty view")

## Display views when data is unavailable

The `EmptyView` property can be set to a view, which will be displayed when the `ItemsSource` property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. This can be a single view, or a view that contains multiple child views. The following XAML example shows the `EmptyView` property set to a view that contains multiple child views:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

The result is that when the data bound collection is `null`, the [`StackLayout`](xref:Xamarin.Forms.StackLayout) and its child views are displayed.

[![Screenshot of a bindable layout empty view with multiple views, on iOS and Android](bindable-layouts-images/emptyview-views.png "Bindable layout empty view")](bindable-layouts-images/emptyview-views-large.png#lightbox "Bindable layout empty view")

Similarly, the `EmptyViewTemplate` can be set to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), which will be displayed when the `ItemsSource` property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The `DataTemplate` can contain a single view, or a view that contains multiple child views. In addition, the `BindingContext` of the `EmptyViewTemplate` will be inherited from the `BindingContext` of the `BindableLayout`. The following XAML example shows the `EmptyViewTemplate` property set to a `DataTemplate` that contains a single view:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

The result is that when the data bound collection is `null`, the [`Label`](xref:Xamarin.Forms.Label) in the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) is displayed:

[![Screenshot of a bindable layout empty view template, on iOS and Android](bindable-layouts-images/emptyviewtemplate.png "Bindable layout empty view template")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "Bindable layout empty view template")

> [!NOTE]
> The `EmptyViewTemplate` property can't be set via a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector).

## Choose an EmptyView at runtime

Views that will be displayed as an `EmptyView` when data is unavailable, can be defined as [`ContentView`](xref:Xamarin.Forms.ContentView) objects in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The `EmptyView` property can then be set to a specific `ContentView`, based on some business logic, at runtime. The following XAML shows an example of this scenario:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

The XAML defines two [`ContentView`](xref:Xamarin.Forms.ContentView) objects in the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), with the [`Switch`](xref:Xamarin.Forms.Switch) object controlling which `ContentView` object will be set as the `EmptyView` property value. When the `Switch` is toggled, the `OnEmptyViewSwitchToggled` event handler executes the `ToggleEmptyView` method:

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

The `ToggleEmptyView` method sets the `EmptyView` property of the `stackLayout` object to one of the two [`ContentView`](xref:Xamarin.Forms.ContentView) objects stored in the [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), based on the value of the [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) property. Then, when the data bound collection is `null`, the `ContentView` object set as the `EmptyView` property is displayed:

[![Screenshot of empty view choice at runtime, on iOS and Android](bindable-layouts-images/emptyview-runtime.png "Bindable layout empty view runtime choice")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "Bindable layout empty view runtime choice")

## Related links

- [Bindable Layout Demo (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Creating a Custom Layout](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
