---
title: "Xamarin.Forms CarouselView Introduction"
description: "CarouselView is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items."
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms CarouselView Introduction

[`CarouselView`](xref:Xamarin.Forms.CarouselView) is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items. By default, `CarouselView` will display its items in a horizontal orientation. A single item will be displayed on screen, with swipe gestures resulting in forwards and backwards navigation through the collection of items. In addition, indicators can be displayed that represent each item in the `CarouselView`:

[![Screenshot of a CarouselView and IndicatorView, on iOS and Android](populate-data-images/indicators.png "IndicatorView circles")](populate-data-images/indicators-large.png#lightbox "IndicatorView circles")

By default, [`CarouselView`](xref:Xamarin.Forms.CarouselView) provides looped access to its collection of items. Therefore, swiping backwards from the first item in the collection will display the last item in the collection. Similarly, swiping forwards from the last item in the collection will return to the first item in the collection.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) shares much of its implementation with [`CollectionView`](xref:Xamarin.Forms.CollectionView). However, the two controls have different use cases. `CollectionView` is typically used to present lists of data of any length, whereas `CarouselView` is typically used to highlight information in a list of limited length.
