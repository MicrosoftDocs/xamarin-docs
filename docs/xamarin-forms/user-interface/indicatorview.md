---
title: "Xamarin.Forms IndicatorView"
description: "The IndicatorView is a control that displays indicators that represent the number of items, and current position, in a CarouselView."
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms IndicatorView

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

The `IndicatorView` is a control that displays indicators that represent the number of items, and current position, in a `CarouselView`:

[![Screenshot of a CarouselView and IndicatorView, on iOS and Android](indicatorview-images/circles.png "IndicatorView circles")](indicatorview-images/circles-large.png#lightbox "IndicatorView circles")

`IndicatorView` defines the following properties:

- `Count`, of type `int`, the number of indicators.
- `HideSingle`, of type `bool`, indicates whether the indicator should be hidden when only one exists. The default value is `true`.
- `IndicatorColor`, of type `Color`, the color of the indicators.
- `IndicatorSize`, of type `double`, the size of the indicators. The default value is 6.0.
- `IndicatorLayout`, of type `Layout<View>`, defines the layout class used to render the `IndicatorView`. This property is set by Xamarin.Forms, and does not typically need to be set by developers.
- `IndicatorTemplate`, of type `DataTemplate`, the template that defines the appearance of each indicator.
- `IndicatorsShape`, of type `IndicatorShape`, the shape of each indicator.
- `ItemsSource`, of type `IEnumerable`, the collection that indicators will be displayed for. This property will automatically be set when the `CarouselView.IndicatorView` property is set.
- `MaximumVisible`, of type `int`, the maximum number of visible indicators. The default value is `int.MaxValue`.
- `Position`, of type `int`, the currently selected indicator index. This property uses a `TwoWay` binding. This property will automatically be set when the `CarouselView.IndicatorView` property is set.
- `SelectedIndicatorColor`, of type `Color`, the color of the indicator that represents the current item in the `CarouselView`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Create an IndicatorView

The following example shows how to instantiate an `IndicatorView` in XAML:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

In this example, the `IndicatorView` is rendered beneath the `CarouselView`, with an indicator for each item in the `CarouselView`. The `IndicatorView` is populated with data by setting the `CarouselView.IndicatorView` property to the `IndicatorView` object. Each indicator is a light gray circle, while the indicator that represents the current item in the `CarouselView` is dark gray.

> [!IMPORTANT]
> Setting the `CarouselView.IndicatorView` property results in the `IndicatorView.Position` property binding to the `CarouselView.Position` property, and the `IndicatorView.ItemsSource` property binding to the `CarouselView.ItemsSource` property.

## Change indicator shape

The `IndicatorView` class has an `IndicatorsShape` property, which determines the shape of the indicators. This property can be set to one of the `IndicatorShape` enumeration members:

- `Circle` specifies that the indicator shapes will be circular. This is the default value of the `IndicatorView.IndicatorsShape` property.
- `Square` indicates that the indicator shapes will be square.

The following example shows an `IndicatorView` configured to use square indicators:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## Change indicator size

The `IndicatorView` class has an `IndicatorSize` property, of type `double`, which determines the size of the indicators in device-independent units. The default value of this property is 6.0.

The following example shows an `IndicatorView` configured to display larger indicators:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## Limit the number of indicators displayed

The `IndicatorView` class has a `MaximumVisible` property, of type `int`, which determines the maximum number of visible indicators.

The following example shows an `IndicatorView` configured to display a maximum of six indicators:

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## Define indicator appearance

The appearance of each indicator can be defined by setting the `IndicatorView.IndicatorTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

The elements specified in the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) define the appearance of each indicator. In this example, each indicator is an [`Image`](xref:Xamarin.Forms.Image) that displays a font icon using the `FontImage` markup extension.

The following screenshots show indicators rendered using a font icon:

[![Screenshot of a templated IndicatorView, on iOS and Android](indicatorview-images/templated.png "Templated IndicatorView")](indicatorview-images/templated-large.png#lightbox "Templated IndicatorView")

For more information about the `FontImage` markup extension, see [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## Related links

- [IndicatorView (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)