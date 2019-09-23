---
title: "Xamarin.Forms CarouselView Introduction"
description: "The CarouselView is a view for presenting data in a carousel-like layout."
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
---

# Xamarin.Forms CarouselView Introduction

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) is a view for presenting information in a carousel-like way.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) is available in Xamarin.Forms 4.3. However, it is currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_Note: As of this writing, the CarouselView still uses the CollectionView flag to enable its functionality._

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) is available on iOS and Android, but some functionality may only be partially available on the Universal Windows Platform.

## When to use CarouselView

While the CarouselView bases much of its implementation off of CollectionView, its purpose is more focused. While the use of CollectionView and CarouselView are at your discretion, carousels in apps are typically used to highlight information and are limited in the total number of items used.
