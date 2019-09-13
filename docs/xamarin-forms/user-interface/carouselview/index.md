---
title: "Xamarin.Forms CarouselView"
description: "The CarouselView is a view for presenting lists of data in a carousel-like layout."
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
---

# Xamarin.Forms CarouselView

![](~/media/shared/preview.png "This API is currently pre-release")

> [!IMPORTANT]
> The CarouselView documentation is under development and some links may refer to CollectionView documentation. In most cases the information should be applicable due to the nature of CarouselView being based upon CollectionView.

## [Introduction](introduction.md)

The [`CarouselView`](xref:Xamarin.Forms.CarouselView) is a view for presenting data in a carousel-like layout. Its implementation is based on that of [`CollectionView`](xref:Xamarin.Forms.CollectionView).

## [Data](../collectionview/populate-data.md)

A [`CarouselView`](xref:Xamarin.Forms.CarouselView) is populated with data by setting its [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) property to any collection that implements `IEnumerable`. The appearance of each item in the list can be defined by setting the [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

## [Layout](layout.md)

By default, a [`CarouselView`](xref:Xamarin.Forms.CarouselView) will display its items in a horizontal list. However, it also has access to the same layouts as CollectionView, including a vertical orientation.

## [Empty views](../collectionview/emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView), an empty view can be specified that provides feedback to the user when no data is available for display. The empty view can be a string, a view, or multiple views.

## [Scrolling](../collectionview/scrolling.md)

When a user swipes to initiate a scroll, the end position of the scroll can be controlled so that items are fully displayed. In addition, [`CarouselView`](xref:Xamarin.Forms.CarouselView) defines two [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) methods, that programmatically scroll items into view. One of the overloads scrolls the item at the specified index into view, while the other scrolls the specified item into view.
