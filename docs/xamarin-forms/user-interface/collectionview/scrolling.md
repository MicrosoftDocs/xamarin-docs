---
title: "Xamarin.Forms CollectionView Scrolling"
description: "When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, CollectionView defines two ScrollTo methods, that programmatically scroll items into view."
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
---

# Xamarin.Forms CollectionView Scrolling

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) defines two [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods, that scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view. Both overloads have additional arguments that can be specified to indicate the group the item belongs to, the exact position of the item after the scroll has completed, and whether to animate the scroll.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) defines a [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) event that is fired when one of the [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods is invoked. The [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) object that accompanies the `ScrollToRequested` event has many properties, including `IsAnimated`, `Index`, `Item`, and `ScrollToPosition`. These properties are set from the arguments specified in the `ScrollTo` method calls.

In addition, [`CollectionView`](xref:Xamarin.Forms.CollectionView) defines a `Scrolled` event that is fired to indicate that scrolling occurred. The `ItemsViewScrolledEventArgs` object that accompanies the `Scrolled` event has many properties. For more information, see [Detect scrolling](#detect-scrolling).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) also defines a `ItemsUpdatingScrollMode` property that represents the scrolling behavior of the `CollectionView` when new items are added to it. For more information about this property, see [Control scroll position when new items are added](#control-scroll-position-when-new-items-are-added).

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. This feature is known as snapping, because items snap to position when scrolling stops. For more information, see [Snap points](#snap-points).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) can also load data incrementally as the user scrolls. For more information, see [Load data incrementally](populate-data.md#load-data-incrementally).

## Detect scrolling

[`CollectionView`](xref:Xamarin.Forms.CollectionView) defines a `Scrolled` event which is fired to indicate that scrolling occurred. The following XAML example shows a `CollectionView` that sets an event handler for the `Scrolled` event:

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

The equivalent C# code is:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

In this code example, the `OnCollectionViewScrolled` event handler is executed when the `Scrolled` event fires:

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

In this example, the `OnCollectionViewScrolled` event handler outputs the values of the `ItemsViewScrolledEventArgs` object that accompanies the event.

> [!IMPORTANT]
> The `Scrolled` event is fired for user initiated scrolls, and for programmatic scrolls.

## Scroll an item at an index into view

The first [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) method overload scrolls the item at the specified index into view. Given a [`CollectionView`](xref:Xamarin.Forms.CollectionView) object named `collectionView`, the following example shows how to scroll the item at index 12 into view:

```csharp
collectionView.ScrollTo(12);
```

Alternatively, an item in grouped data can be scrolled into view by specifying the item and group indexes. The following example shows how to scroll the third item in the second group into view:

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> The [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) event is fired when the [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) method is invoked.

## Scroll an item into view

The second [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) method overload scrolls the specified item into view. Given a [`CollectionView`](xref:Xamarin.Forms.CollectionView) object named `collectionView`, the following example shows how to scroll the Proboscis Monkey item into view:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

Alternatively, an item in grouped data can be scrolled into view by specifying the item and the group. The following example shows how to scroll the Proboscis Monkey item in the Monkeys group into view:

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> The [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) event is fired when the [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) method is invoked.

## Disable scroll animation

A scrolling animation is displayed when scrolling an item into view. However, this animation can be disabled by setting the `animate` argument of the `ScrollTo` method to `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## Control scroll position

When scrolling an item into view, the exact position of the item after the scroll has completed can be specified with the `position` argument of the [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods. This argument accepts a [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) enumeration member.

### MakeVisible

The [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) member indicates that the item should be scrolled until it's visible in the view:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

This example code results in the minimal scrolling required to scroll the item into view:

[![Screenshot of a CollectionView vertical list with an item scrolled into view, on iOS and Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView vertical list with scrolled item")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView vertical list with scrolled item")

> [!NOTE]
> The [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) member is used by default, if the `position` argument is not specified when calling the `ScrollTo` method.

### Start

The [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) member indicates that the item should be scrolled to the start of the view:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

This example code results in the item being scrolled to the start of the view:

[![Screenshot of a CollectionView vertical list with an item scrolled into view, on iOS and Android](scrolling-images/scrolltoposition-start.png "CollectionView vertical list with scrolled item")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView vertical list with scrolled item")

### Center

The [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) member indicates that the item should be scrolled to the center of the view:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

This example code results in the item being scrolled to the center of the view:

[![Screenshot of a CollectionView vertical list with an item scrolled into view, on iOS and Android](scrolling-images/scrolltoposition-center.png "CollectionView vertical list with scrolled item")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView vertical list with scrolled item")

### End

The [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) member indicates that the item should be scrolled to the end of the view:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

This example code results in the item being scrolled to the end of the view:

[![Screenshot of a CollectionView vertical list with an item scrolled into view, on iOS and Android](scrolling-images/scrolltoposition-end.png "CollectionView vertical list with scrolled item")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView vertical list with scrolled item")

## Control scroll position when new items are added

[`CollectionView`](xref:Xamarin.Forms.CollectionView) defines a `ItemsUpdatingScrollMode` property, which is backed by a bindable property. This property gets or sets a `ItemsUpdatingScrollMode` enumeration value that represents the scrolling behavior of the `CollectionView` when new items are added to it. The `ItemsUpdatingScrollMode` enumeration defines the following members:

- `KeepItemsInView` adjusts the scroll offset to keep the first visible item displayed when new items are added.
- `KeepScrollOffset` maintains the scroll offset relative to the beginning of the list when new items are added.
- `KeepLastItemInView` adjusts the scroll offset to keep the last item visible when new items are added.

The default value of the `ItemsUpdatingScrollMode` property is `KeepItemsInView`. Therefore, when new items are added to a [`CollectionView`](xref:Xamarin.Forms.CollectionView) the first visible item in the list will remain displayed. To ensure that newly added items are always visible at the bottom of the list, the `ItemsUpdatingScrollMode` property should be set to `KeepLastItemInView`:

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

The equivalent C# code is:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## Scroll bar visibility

[`CollectionView`](xref:Xamarin.Forms.CollectionView) defines `HorizontalScrollBarVisibility` and `VerticalScrollBarVisibility` properties, which are backed by bindable properties. These properties get or set a [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) enumeration value that represents when the horizontal, or vertical, scroll bar is visible. The `ScrollBarVisibility` enumeration defines the following members:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) indicates the default scroll bar behavior for the platform, and is the default value for the `HorizontalScrollBarVisibility` and `VerticalScrollBarVisibility` properties.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) indicates that scroll bars will be visible, even when the content fits in the view.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) indicates that scroll bars will not be visible, even if the content doesn't fit in the view.

## Snap points

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. This feature is known as snapping, because items snap to position when scrolling stops, and is controlled by the following properties from the [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) class:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType), of type [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType), specifies the behavior of snap points when scrolling.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), of type [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), specifies how snap points are aligned with items.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

> [!NOTE]
> When snapping occurs, it will occur in the direction that produces the least amount of motion.

### Snap points type

The [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) enumeration defines the following members:

- `None` indicates that scrolling does not snap to items.
- `Mandatory` indicates that content always snaps to the closest snap point to where scrolling would naturally stop, along the direction of inertia.
- `MandatorySingle` indicates the same behavior as `Mandatory`, but only scrolls one item at a time.

By default, the [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) property is set to `SnapPointsType.None`, which ensures that scrolling does not snap items, as shown in the following screenshots:

[![Screenshot of a CollectionView vertical list without snap points, on iOS and Android](scrolling-images/snappoints-none.png "CollectionView vertical list without snap points")](scrolling-images/snappoints-none-large.png#lightbox "CollectionView vertical list without snap points")

### Snap points alignment

The [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) enumeration defines `Start`, `Center`, and `End` members.

> [!IMPORTANT]
> The value of the [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) property is only respected when the [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) property is set to `Mandatory`, or `MandatorySingle`.

#### Start

The `SnapPointsAlignment.Start` member indicates that snap points are aligned with the leading edge of items.

By default, the [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) property is set to `SnapPointsAlignment.Start`. However, for completeness, the following XAML example shows how to set this enumeration member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

The equivalent C# code is:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

When a user swipes to initiate a scroll, the top item will be aligned with the top of the view:

[![Screenshot of a CollectionView vertical list with start snap points, on iOS and Android](scrolling-images/snappoints-start.png "CollectionView vertical list with start snap points")](scrolling-images/snappoints-start-large.png#lightbox "CollectionView vertical list with start snap points")

#### Center

The `SnapPointsAlignment.Center` member indicates that snap points are aligned with the center of items. The following XAML example shows how to set this enumeration member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

The equivalent C# code is:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

When a user swipes to initiate a scroll, the top item will be center aligned at the top of the view:

[![Screenshot of a CollectionView vertical list with center snap points, on iOS and Android](scrolling-images/snappoints-center.png "CollectionView vertical list with center snap points")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView vertical list with center snap points")

#### End

The `SnapPointsAlignment.End` member indicates that snap points are aligned with the trailing edge of items. The following XAML example shows how to set this enumeration member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

The equivalent C# code is:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

When a user swipes to initiate a scroll, the bottom item will be aligned with the bottom of the view:

[![Screenshot of a CollectionView vertical list with end snap points, on iOS and Android](scrolling-images/snappoints-end.png "CollectionView vertical list with end snap points")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView vertical list with end snap points")

## Related links

- [CollectionView (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
