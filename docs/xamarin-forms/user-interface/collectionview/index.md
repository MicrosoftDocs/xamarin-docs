---
title: "Xamarin.Forms CollectionView"
description: "The CollectionView is a flexible and performant view for presenting lists of data using different layout specifications."
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
---

# Xamarin.Forms CollectionView

![Preview](~/media/shared/preview.png)

> [!IMPORTANT]
> The `CollectionView` is currently a preview, and lacks some of its planned functionality. In addition, the API may change as the implementation is completed.

`CollectionView` is a view for presenting lists of data using different layout specifications. It aims to provide a more flexible, and performant alternative to [`ListView`](xref:Xamarin.Forms.ListView). While the `CollectionView` and `ListView` APIs are similar, there are some notable differences:

- `CollectionView` has a flexible layout model, which allows data to be presented vertically or horizontally, in a list or a grid.
- `CollectionView` has no concept of cells. Instead, a data template is used to define the appearance of each item of data in the list.
- `CollectionView` automatically utilizes the virtualization provided by the underlying native controls.
- `CollectionView` reduces the API surface of [`ListView`](xref:Xamarin.Forms.ListView). Many properties and events from `ListView` are not present in `CollectionView`.
- `CollectionView` does not include built-in separators.

`CollectionView` is available in the Xamarin.Forms 4.0 pre-releases. However, it is currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` is only available on iOS and Android.

## [Populate CollectionView with data](populate-data.md)

A `CollectionView` is populated with data by setting its `ItemsSource` property to any collection that implements `IEnumerable`. The appearance of each item in the list can be defined by setting the `ItemTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## [Specify CollectionView layout](layout.md)

By default, a `CollectionView` will display its items in a vertical list. However, vertical and horizontal lists and grids can be specified.

## [Set CollectionView selection mode](selection.md)

By default, `CollectionView` selection is disabled. However, single and multiple selection can be enabled.

## [Display an EmptyView when data is unavailable](emptyview.md)

In `CollectionView`, an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views.

## [Scroll an item into view](scrolling.md)

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, `CollectionView` defines two `ScrollTo` methods, that programmatically scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view.
