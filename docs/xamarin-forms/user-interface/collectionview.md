---
title: "Xamarin.Forms CollectionView"
description: "The CollectionView is a flexible and performant view for presenting lists of data using different layout specifications."
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
---

# Xamarin.Forms CollectionView

![Preview](~/media/shared/preview.png)

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` is a view for presenting lists of data using different layout specifications. It aims to provide a more flexible, and performant alternative to `ListView`. While the `CollectionView` and `ListView` APIs are similar, there are some notable differences:

- `CollectionView` has no concept of cells. Instead, data templates are used to define the appearance of each item of data in the list.
- `CollectionView` reduces the API surface of `ListView`. Many properties and events from `ListView` are not present in `CollectionView`.
- `CollectionView` has a flexible layout model, which allows data to be presented vertically or horizontally, in a list or a grid.

A `CollectionView` is consumed by setting its `ItemsSource` property to any collection that implements `IEnumerable`, and its `ItemTemplate` property to a `DataTemplate` that defines each list item appearance. In addition, its `ItemsLayout` property must be set to an `ItemsLayout` class, to define the layout specification for the list.

> [!IMPORTANT]
> The `CollectionView` is currently an early preview, and lacks much of its planned functionality. In addition, the API will change as the implementation is completed.

`CollectionView` is available in Xamarin.Forms 4.0-pre1. However, it is currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

## Populating a CollectionView with data

A `CollectionView` is populated with data by setting its `ItemSource` property to any collection that implements `IEnumerable`. Items can be added in XAML by initializing the `ItemsSource` property from an array of items:

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> Note that the `x:Array` element requires a `Type` attribute indicating the type of the items in the array.

In addition, a `CollectionView` can be populated with data by using data binding to bind its `ItemsSource` property to a `IEnumerable` collection. In XAML this is achieved with the `Binding` markup extension:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

In this example, the `ItemsSource` property data binds to the `Monkeys` property of the connected view model, which returns a `IList<Monkey>` collection. The following code example shows the `Monkey` class, which contains four properties:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

## Providing a view for the empty state

When the `CollectionView` doesn't have any data to display, a suitable message can be displayed to the user. This is accomplished by setting the `CollectionView.EmptyView` property to an `object` that will be displayed when the collection specified by the `ItemSource` property is empty:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}">
    <CollectionView.EmptyView>
        <Label Text="No items to display" />
    </CollectionView.EmptyView>
</CollectionView>
```

The `EmptyView` property can be set to a `string`, a binding, or a view, and so can include interactive content if required.

In addition, the `EmptyViewTemplate` property can be set to a `DataTemplate` that will be used to format the `EmptyView`.

## Defining list item appearance

The appearance of the data for each item in the `CollectionView` can be defined by setting the `CollectionView.ItemTemplate` property to a `DataTemplate`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

In this example, the `DataTemplate` contains a `Grid` with an `Image`, and two `Label` instances, that all bind to properties of the `Monkey` object.

For more information about data templates, see [Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## Specifying a layout

By default, a `CollectionView` will display its items in a vertical list. However, any of the following layout specifications can be used:

- Vertical list – a single column list that grows vertically as new items are added.
- Horizontal list – a single row list that grows horizontally as new items are added.
- Vertical grid – a multi-column grid that grows vertically as new items are added.
- Horizontal grid – a multi-row grid that grows horizontally as new items are added.

These layouts can be specified by setting the `CollectionView.ItemsLayout` property to a `ItemsLayout` class, with the `ListItemsLayout` class providing a list layout, and the `GridItemsLayout` class providing a grid layout.

The `ItemsLayout` class defines the `Orientation` property, of type `ItemsLayoutOrientation`. The `ItemsLayoutOrientation` enumeration defines `Vertical` and `Horizontal` member values.

The `ListItemsLayout` class inherits from the `ItemsLayout` class, and defines static `VerticalList` and `HorizontalList` members. These members can be used to create vertical or horizontal lists, respectively. Alternatively, a `ListItemsLayout` instance can be created, specifying an `ItemsLayoutOrientation` enumeration member as an argument.

The `GridItemsLayout` class inherits from the `ItemsLayout` class, and defines a `Span` property, of type `int`, that represents the number of columns or rows to display in the grid. The default value of the `Span` property is 1, and its value must always be greater than or equal to 1.

### Vertical list

By default, a `CollectionView` will display its items in a vertical list layout. Therefore, it's not necessary to set the `ItemsLayout` property to use this layout:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

However, for completeness, a `CollectionView` can be set to display its items in a vertical list by setting its `ItemsLayout` to the static `ListItemsLayout.VerticalList` member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

Alternatively, this can also be accomplished by setting the `ItemsLayout` property to an instance of the `ListItemsLayout` class, specifying the `Vertical` `ItemsLayoutOrientation` enumeration member as an argument:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

This results in a single column list, which grows vertically as new items are added:

[![CollectionView vertical list layout](collectionview-images/vertical-list.png "CollectionView vertical list layout")](collectionview-images/vertical-list-large.png#lightbox)

### Horizontal list

A `CollectionView` can display its items in a horizontal list by setting its `ItemsLayout` property to the static `ListItemsLayout.HorizontalList` member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Alternatively, this can also be accomplished by setting the `ItemsLayout` property to an instance of the `ListItemsLayout` class, specifying the `Horizontal` `ItemsLayoutOrientation` enumeration member as an argument:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

This results in a single row list, which grows horizontally as new items are added:

[![CollectionView horizontal list layout](collectionview-images/horizontal-list.png "CollectionView horizontal list layout")](collectionview-images/horizontal-list-large.png#lightbox)

### Vertical grid

A `CollectionView` can display its items in a vertical grid by setting its `ItemsLayout` property to a `GridItemsLayout` instance whose `Orientation` property is set to `Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

By default, a vertical `GridItemsLayout` will display items in a single column. However, this example sets the `GridItemsLayout.Span` property to 2. This results in a two-column grid, which grows vertically as new items are added:

[![CollectionView vertical grid layout](collectionview-images/vertical-grid.png "CollectionView vertical grid layout")](collectionview-images/vertical-grid-large.png#lightbox)

### Horizontal grid

A `CollectionView` can display its items in a horizontal grid by setting its `ItemsLayout` property to a `GridItemsLayout` instance whose `Orientation` property is set to `Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

By default, a horizontal `GridItemsLayout` will display items in a single row. However, this example sets the `GridItemsLayout.Span` property to 4. This results in a four-row grid, which grows horizontally as new items are added:

[![CollectionView horizontal grid layout](collectionview-images/horizontal-grid.png "CollectionView horizontal grid layout")](collectionview-images/horizontal-grid-large.png#lightbox)

## Scrolling

`CollectionView` can scroll requested items into view with the `ScrollTo` method. This method scrolls the item at the specified index into view:

```csharp
_collectionView.ScrollTo(12);
```

Alternatively, an overload of this method can be used to scroll the specified item into view:

```csharp
var viewModel = BindingContext as MonkeysViewModel;
var monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
_collectionView.ScrollTo(monkey);
```

Both overloads allow additional arguments to be specified that indicate the group the item is in, the exact position of the item after the scroll has completed (start, center, end), and whether to animate the scroll.

## Related links

- [CollectionView (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
