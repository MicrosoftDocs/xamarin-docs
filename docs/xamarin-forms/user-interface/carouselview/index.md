---
title: "Xamarin.Forms CarouselView"
description: "The CarouselView is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items."
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms CarouselView

## [Introduction](introduction.md)

The [`CarouselView`](xref:Xamarin.Forms.CarouselView) is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items.

## [Data](populate-data.md)

A [`CarouselView`](xref:Xamarin.Forms.CarouselView) is populated with data by setting its [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property to any collection that implements `IEnumerable`. The appearance of each item can be defined by setting the [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## [Layout](layout.md)

By default, a [`CarouselView`](xref:Xamarin.Forms.CarouselView) will display its items in a horizontal list. However, it also has access to the same layouts as CollectionView, including a vertical orientation.

## [Interaction](interaction.md)

The currently displayed item in a [`CarouselView`](xref:Xamarin.Forms.CarouselView) can be accessed through the `CurrentItem` and `Position` properties.

## [Empty views](emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView), an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views.

## [Scrolling](scrolling.md)

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, [`CarouselView`](xref:Xamarin.Forms.CarouselView) defines two [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods, that programmatically scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view.
