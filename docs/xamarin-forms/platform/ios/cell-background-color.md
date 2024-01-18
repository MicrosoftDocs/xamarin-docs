---
title: "Cell Background Color on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that sets the default background color of cells on iOS."
ms.service: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Cell Background Color on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific sets the default background color of [`Cell`](xref:Xamarin.Forms.Cell) instances. It's consumed in XAML by setting the `Cell.DefaultBackgroundColor` bindable property to a [`Color`](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

The `ListView.On<iOS>` method specifies that this platform-specific will only run on iOS. The `Cell.SetDefaultBackgroundColor` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, sets the cell background color to a specified [`Color`](xref:Xamarin.Forms.Color). In addition, the `Cell.DefaultBackgroundColor` method can be used to retrieve the current cell background color.

The result is that the background color in a [`Cell`](xref:Xamarin.Forms.Cell) can be set to a specific [`Color`](xref:Xamarin.Forms.Color):

[![Screenshot of the Teal group header cells, on iOS](cell-background-color-images/group-header-cell-color.png "ListView with teal group header cells")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView with teal group header cells")

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)