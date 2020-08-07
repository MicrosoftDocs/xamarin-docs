---
title: "Xamarin.Forms CarouselView Introduction"
description: "CarouselView is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items."
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms CarouselView Introduction

![Pre-release API](~/media/shared/preview.png)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) is a view for presenting data in a scrollable layout, where users can swipe to move through a collection of items. By default, `CarouselView` will display its items in a horizontal orientation. A single item will be displayed on screen, with swipe gestures resulting in forwards and backwards navigation through the collection of items. In addition, indicators can be displayed that represent each item in the `CarouselView`:

[![Screenshot of a CarouselView and IndicatorView, on iOS and Android](populate-data-images/indicators.png "IndicatorView circles")](populate-data-images/indicators-large.png#lightbox "IndicatorView circles")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) is available in Xamarin.Forms 4.3. However, it's currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) is available on iOS and Android, but some functionality may only be partially available on the Universal Windows Platform.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) shares much of its implementation with [`CollectionView`](xref:Xamarin.Forms.CollectionView). However, the two controls have different use cases. `CollectionView` is typically used to present lists of data of any length, whereas `CarouselView` is typically used to highlight information in a list of limited length.
