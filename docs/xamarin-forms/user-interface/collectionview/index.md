---
title: "Xamarin.Forms CollectionView"
description: "The CollectionView is a flexible and performant view for presenting lists of data using different layout specifications."
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
---

# Xamarin.Forms CollectionView

![](~/media/shared/preview.png "This API is currently pre-release")

## [Introduction](introduction.md)

The `CollectionView` is a flexible and performant view for presenting lists of data using different layout specifications.

## [Data](populate-data.md)

A `CollectionView` is populated with data by setting its `ItemsSource` property to any collection that implements `IEnumerable`. The appearance of each item in the list can be defined by setting the `ItemTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## [Layout](layout.md)

By default, a `CollectionView` will display its items in a vertical list. However, vertical and horizontal lists and grids can be specified.

## [Selection](selection.md)

By default, `CollectionView` selection is disabled. However, single and multiple selection can be enabled.

## [Empty views](emptyview.md)

In `CollectionView`, an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views.

## [Scrolling](scrolling.md)

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, `CollectionView` defines two `ScrollTo` methods, that programmatically scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view.
