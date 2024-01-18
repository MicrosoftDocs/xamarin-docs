---
title: "Xamarin.Forms CarouselView EmptyView"
description: "In CarouselView, an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views."
ms.service: xamarin
ms.assetid: C6DEE1A9-63FC-4889-BC77-F401D5D7DF32
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms CarouselView EmptyView

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) defines the following properties that can be used to provide user feedback when there's no data to display:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView), of type `object`, the string, binding, or view that will be displayed when the [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The default value is `null`.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate), of type [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), the template to use to format the specified `EmptyView`. The default value is `null`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

The main usage scenarios for setting the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property are displaying user feedback when a filtering operation on a [`CarouselView`](xref:Xamarin.Forms.CarouselView) yields no data, and displaying user feedback while data is being retrieved from a web service.

> [!NOTE]
> The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property can be set to a view that includes interactive content if required.

For more information about data templates, see [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## Display a string when data is unavailable

The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property can be set to a string, which will be displayed when the [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The following XAML shows an example of this scenario:

```xaml
<CarouselView ItemsSource="{Binding EmptyMonkeys}"
              EmptyView="No items to display." />
```

The equivalent C# code is:

```csharp
CarouselView carouselView = new CarouselView
{
    EmptyView = "No items to display."
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

The result is that, because the data bound collection is `null`, the string set as the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property value is displayed.

## Display views when data is unavailable

The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property can be set to a view, which will be displayed when the [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. This can be a single view, or a view that contains multiple child views. The following XAML example shows the `EmptyView` property set to a view that contains multiple child views:

```xaml
<StackLayout Margin="20">
    <SearchBar SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <ContentView>
                <StackLayout HorizontalOptions="CenterAndExpand"
                             VerticalOptions="CenterAndExpand">
                    <Label Text="No results matched your filter."
                           Margin="10,25,10,10"
                           FontAttributes="Bold"
                           FontSize="18"
                           HorizontalOptions="Fill"
                           HorizontalTextAlignment="Center" />
                    <Label Text="Try a broader filter?"
                           FontAttributes="Italic"
                           FontSize="12"
                           HorizontalOptions="Fill"
                           HorizontalTextAlignment="Center" />
                </StackLayout>
            </ContentView>
        </CarouselView.EmptyView>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

In this example, what looks like a redundant [`ContentView`](xref:Xamarin.Forms) has been added as the root element of the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView). This is because internally, the `EmptyView` is added to a native container that doesn't provide any context for Xamarin.Forms layout. Therefore, to position the views that comprise your `EmptyView`, you must add a root layout, whose child is a layout that can position itself within the root layout.

The equivalent C# code is:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new ContentView
    {
        Content = new StackLayout
        {
            Children =
            {
                new Label { Text = "No results matched your filter.", ... },
                new Label { Text = "Try a broader filter?", ... }
            }
        }
    }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

When the [`SearchBar`](xref:Xamarin.Forms.SearchBar) executes the `FilterCommand`, the collection displayed by the [`CarouselView`](xref:Xamarin.Forms.CarouselView) is filtered for the search term stored in the [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) property. If the filtering operation yields no data, the [`StackLayout`](xref:Xamarin.Forms.StackLayout) set as the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property value is displayed.

## Display a templated custom type when data is unavailable

The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property can be set to a custom type, whose template is displayed when the [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property is `null`, or when the collection specified by the `ItemsSource` property is `null` or empty. The [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property can be set to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) that defines the appearance of the `EmptyView`. The following XAML shows an example of this scenario:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <controls:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CarouselView.EmptyView>
        <CarouselView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CarouselView.EmptyViewTemplate>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

The equivalent C# code is:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

The `FilterData` type defines a `Filter` property, and a corresponding [`BindableProperty`](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property is set to a `FilterData` object, and the `Filter` property data binds to the [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) property. When the [`SearchBar`](xref:Xamarin.Forms.SearchBar) executes the `FilterCommand`, the collection displayed by the [`CarouselView`](xref:Xamarin.Forms.CarouselView) is filtered for the search term stored in the `Filter` property. If the filtering operation yields no data, the [`Label`](xref:Xamarin.Forms.Label) defined in the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), that's set as the [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property value, is displayed.

> [!NOTE]
> When displaying a templated custom type when data is unavailable, the [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property can be set to a view that contains multiple child views.

## Choose an EmptyView at runtime

Views that will be displayed as an [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) when data is unavailable, can be defined as [`ContentView`](xref:Xamarin.Forms.ContentView) objects in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The `EmptyView` property can then be set to a specific `ContentView`, based on some business logic, at runtime. The following XAML example shows an example of this scenario:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:viewmodels="clr-namespace:CarouselViewDemos.ViewModels"
             x:Class="CarouselViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.BindingContext>
        <viewmodels:MonkeysViewModel />
    </ContentPage.BindingContext>
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <SearchBar SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CarouselView x:Name="carouselView"
                      ItemsSource="{Binding Monkeys}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

This XAML defines two [`ContentView`](xref:Xamarin.Forms.ContentView) objects in the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), with the [`Switch`](xref:Xamarin.Forms.Switch) object controlling which `ContentView` object will be set as the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property value. When the [`Switch`](xref:Xamarin.Forms.Switch) is toggled, the `OnEmptyViewSwitchToggled` event handler executes the `ToggleEmptyView` method:

```csharp
void ToggleEmptyView(bool isToggled)
{
    carouselView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

The `ToggleEmptyView` method sets the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property of the `carouselView` object to one of the two [`ContentView`](xref:Xamarin.Forms.ContentView) objects stored in the [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), based on the value of the [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) property. When the [`SearchBar`](xref:Xamarin.Forms.SearchBar) executes the `FilterCommand`, the collection displayed by the [`CarouselView`](xref:Xamarin.Forms.CarouselView) is filtered for the search term stored in the [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) property. If the filtering operation yields no data, the `ContentView` object set as the `EmptyView` property is displayed.

For more information about resource dictionaries, see [Xamarin.Forms Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md).

## Choose an EmptyViewTemplate at runtime

The appearance of the [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) can be chosen at runtime, based on its value, by setting the [`CarouselView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property to a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) object:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <CarouselView ItemsSource="{Binding Monkeys}"
                      EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                      EmptyViewTemplate="{StaticResource SearchSelector}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

The equivalent C# code is:

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView()
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

The [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) property is set to the [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) property, and the [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property is set to a `SearchTermDataTemplateSelector` object.

When the [`SearchBar`](xref:Xamarin.Forms.SearchBar) executes the `FilterCommand`, the collection displayed by the [`CarouselView`](xref:Xamarin.Forms.CarouselView) is filtered for the search term stored in the [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) property. If the filtering operation yields no data, the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) chosen by the `SearchTermDataTemplateSelector` object is set as the [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) property and displayed.

The following example shows the `SearchTermDataTemplateSelector` class:

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

The `SearchTermTemplateSelector` class defines `DefaultTemplate` and `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) properties that are set to different data templates. The `OnSelectTemplate` override returns `DefaultTemplate`, which displays a message to the user, when the search query isn't equal to "xamarin". When the search query is equal to "xamarin", the `OnSelectTemplate` override returns `OtherTemplate`, which displays a basic message to the user.

For more information about data template selectors, see [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## Related links

- [CarouselView (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
