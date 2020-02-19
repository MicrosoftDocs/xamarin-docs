---
title: "Xamarin.Forms IndicatorView"
description: "The IndicatorView is a control that displays indicators that represent the number of items, and current position, in a CarouselView."
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
---

# Xamarin.Forms IndicatorView

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

The `IndicatorView` is a control that displays indicators that represent the number of items, and current position, in a `CarouselView`:

[![Screenshot of a CarouselView and IndicatorView, on iOS and Android](indicatorview-images/circles.png "IndicatorView circles")](indicatorview-images/circles-large.png#lightbox "IndicatorView circles")

`IndicatorView` is available in Xamarin.Forms 4.4 on the iOS and Android platforms. However, it's currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` defines the following properties:

- `Count`, of type `int`, the number of indicators.
- `HideSingle`, of type `bool`, indicates whether the indicator should be hidden when only one exists. The default value is `true`.
- `IndicatorColor`, of type `Color`, the color of the indicators.
- `IndicatorSize`, of type `double`, the size of the indicators. The default value is 6.0.
- `IndicatorLayout`, of type `Layout<View>`, defines the layout class used to render the `IndicatorView`. This property is set by Xamarin.Forms, and does not typically need to be set by developers.
- `IndicatorTemplate`, of type `DataTemplate`, the template that defines the appearance of each indicator.
- `IndicatorsShape`, of type `IndicatorShape`, the shape of each indicator.
- `ItemsSource`, of type `IEnumerable`, the collection that indicators will be displayed for. This property will automatically be set when the `ItemsSourceBy` attached property is set.
- `ItemsSourceBy`, of type `VisualElement`, the `CarouselView` object to display indicators for. This is an attached property.
- `MaximumVisible`, of type `int`, the maximum number of visible indicators. The default value is `int.MaxValue`.
- `Position`, of type `int`, the currently selected indicator index. This property uses a `TwoWay` binding. This property will automatically be set when the `ItemsSourceBy` attached property is set.
- `SelectedIndicatorColor`, of type `Color`, the color of the indicator that represents the current item in the `CarouselView`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Create an IndicatorView

The following example shows how to instantiate an `IndicatorView` in XAML:

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView IndicatorView.ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

In this example, the `IndicatorView` is rendered beneath the `CarouselView`, with an indicator for each item in the `CarouselView`. The `IndicatorView` is populated with data by setting the `ItemsSourceBy` attached property to the `CarouselView` object. Each indicator is a light gray circle, while the indicator that represents the current item in the `CarouselView` is dark gray.

> [!IMPORTANT]
> Setting the `ItemsSourceBy` attached property results in the `Position` property binding to the `CarouselView.Position` property, and the `ItemsSource` property binding to the `CarouselView.ItemsSource` property.

## Change indicator shape

The `IndicatorView` class has a `IndicatorsShape` property, which indicates the shape of the indicators. This property can be set to one of the `IndicatorShape` enumeration members:

- `Circle` specifies that the indicator shapes will be circular. This is the default value of the `IndicatorView.IndicatorsShape` property.
- `Square` indicates that the indicator shapes will be square.

The following example shows an `IndicatorView` configured to use square indicators:

```xaml
<IndicatorView IndicatorsShape="Square"
               IndicatorView.ItemsSourceBy="carouselView"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## Define indicator appearance

The appearance of each indicator can be defined by setting the `IndicatorView.IndicatorTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView IndicatorView.ItemsSourceBy="carouselView"
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

- [IndicatorView (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
