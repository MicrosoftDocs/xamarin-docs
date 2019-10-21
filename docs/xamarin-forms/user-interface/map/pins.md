---
title: "Xamarin.Forms Map pins"
description: "This article explains how to create pins on a Xamarin.Forms map."
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/23/2019
---

# Xamarin.Forms Map Pins

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

The Xamarin.Forms `Maps` control allows locations to be marked with `Pin` objects. A `Pin` is a map marker that opens an information window when clicked or tapped.

The `Pin` class has the following properties:

- `Type` is an `PinType` enum value: Generic, Place, SavedPin, or SearchResult.
- `Position` is a `Position` instance containing the latitude and longitude of the pin.
- `Label` is a `string` that is typically displayed as the pin title.
- `Address` is a `string` that will be displayed in the info window. It can be any `string` content, not just an address.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means the `Pin` can be the target of data bindings. For more information, see [Create pins with data binding](#create-pins-with-data-binding).

## Create map pins

A `Pin` instance can be created in code and added to a map:

```csharp
Pin pin1 = new Pin
{
    Type = PinType.Place,
    Position = new Position(47.6368678, -122.137305),
    Label = "Example Pin 1",
    Address = "Example custom details..."
};
map.Pins.Add(pin1);
```

> [!NOTE]
> The `PinType` value affects how pins are rendered depending on the platform. To customize the appearance of a pin, you must create a custom renderer. For more information, see [Customizing a map pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

## Create pins with data binding

The [`Map`](xref:Xamarin.Forms.Maps.Map) class exposes the following properties:

- `ItemsSource` – specifies the collection of `IEnumerable` items to be displayed.
- `ItemTemplate` – specifies the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) to apply to each item in the collection of displayed items.
- `ItemTemplateSelector` – specifies the [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) that will be used to choose a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) for an item at runtime.

> [!NOTE]
> The `ItemTemplate` property takes precedence when both the `ItemTemplate` and `ItemTemplateSelector` properties are set.

A [`Map`](xref:Xamarin.Forms.Maps.Map) can be populated with data by using data binding to bind its `ItemsSource` property to an `IEnumerable` collection:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

The `ItemsSource` property data binds to the `Locations` property of the connected view model, which returns an `ObservableCollection` of `Location` objects, which is a custom type. Each `Location` object defines `Address` and `Description` properties, of type `string`, and a `Position` property, of type [`Position`](xref:Xamarin.Forms.Maps.Position).

The appearance of each item in the `IEnumerable` collection is defined by setting the `ItemTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) that contains a [`Pin`](xref:Xamarin.Forms.Maps.Pin) object that data binds to appropriate properties.

The following screenshots show a [`Map`](xref:Xamarin.Forms.Maps.Map) displaying a [`Pin`](xref:Xamarin.Forms.Maps.Pin) collection using data binding:

[![Screenshot of map with data bound pins, on iOS and Android](map-images/pins-itemssource.png "Map with data bound pins")](map-images/pins-itemssource-large.png#lightbox "Map with data bound pins")

### Choose item appearance at runtime

The appearance of each item in the `IEnumerable` collection can be chosen at runtime, based on the item value, by setting the `ItemTemplateSelector` property to a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector):

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

The following example shows the `MapItemTemplateSelector` class:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

The `MapItemTemplateSelector` class defines `DefaultTemplate` and `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) properties that are set to different data templates. The `OnSelectTemplate` method returns the `XamarinTemplate`, which displays "Xamarin" as a label when a `Pin` is tapped, when the item has an address that contains "San Francisco". When the item doesn't have an address that contains "San Francisco", the `OnSelectTemplate` method returns the `DefaultTemplate`.

For more information about data template selectors, see [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## Pin events

The `Pin` class provides two events:

- `MarkerClicked` is fired when the pin is clicked or tapped.
- `InfoWindowClicked` is fired when the info window is clicked or tapped.

Event handlers can be attached to a pin in code:

```csharp
public class PinEvents: ContentPage
{
    public PinEvents()
    {
        // ...

        pin1.MarkerClicked += OnMarkerClickedAsync;
        pin1.InfoWindowClicked += OnInfoWindowClickedAsync;
    }

    async void OnMarkerClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
    }

    async void OnInfoWindowClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
    }
}
```

## Related links

- [Maps Sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Map Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
