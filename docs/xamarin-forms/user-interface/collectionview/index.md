---
title: "Xamarin.Forms CollectionView"
description: "The CollectionView is a flexible and performant view for presenting lists of data using different layout specifications."
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms CollectionView

## [Introduction](introduction.md)

The [`CollectionView`](xref:Xamarin.Forms.CollectionView) is a flexible and performant view for presenting lists of data using different layout specifications.

## [Data](populate-data.md)

A [`CollectionView`](xref:Xamarin.Forms.CollectionView) is populated with data by setting its [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property to any collection that implements `IEnumerable`. The appearance of each item in the list can be defined by setting the [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## [Layout](layout.md)

By default, a [`CollectionView`](xref:Xamarin.Forms.CollectionView) will display its items in a vertical list. However, vertical and horizontal lists and grids can be specified.

## [Selection](selection.md)

By default, [`CollectionView`](xref:Xamarin.Forms.CollectionView) selection is disabled. However, single and multiple selection can be enabled.

## [Empty views](emptyview.md)

In [`CollectionView`](xref:Xamarin.Forms.CollectionView), an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views.

## [Scrolling](scrolling.md)

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, [`CollectionView`](xref:Xamarin.Forms.CollectionView) defines two [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods, that programmatically scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view.

## [Grouping](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) can display correctly grouped data by setting its `IsGrouped` property to `true`.
