---
title: "Xamarin.Forms CollectionView Introduction"
description: "The CollectionView is a flexible and performant view for presenting lists of data using different layout specifications."
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
---

# Xamarin.Forms CollectionView Introduction

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) is a view for presenting lists of data using different layout specifications. It aims to provide a more flexible, and performant alternative to [`ListView`](xref:Xamarin.Forms.ListView). For example, the following screenshots show a `CollectionView` that uses a two column vertical grid, and which allows multiple selection:

[![Screenshot of a CollectionView vertical grid layout, on iOS and Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertical grid layout with multiple selection")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertical grid layout with multiple selection")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) is available in Xamarin.Forms 4.0. However, it is currently experimental and can only be used by adding the following line of code to your `AppDelegate` class on iOS, or to your `MainActivity` class on Android, before calling `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) is only available on iOS and Android.

## CollectionView and ListView differences

While the [`CollectionView`](xref:Xamarin.Forms.CollectionView) and [`ListView`](xref:Xamarin.Forms.ListView) APIs are similar, there are some notable differences:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) has a flexible layout model, which allows data to be presented vertically or horizontally, in a list or a grid.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) supports single and multiple selection.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) has no concept of cells. Instead, a data template is used to define the appearance of each item of data in the list.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) automatically utilizes the virtualization provided by the underlying native controls.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) reduces the API surface of [`ListView`](xref:Xamarin.Forms.ListView). Many properties and events from [`ListView`](xref:Xamarin.Forms.ListView) are not present in `CollectionView`.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) does not include built-in separators.

## Move from ListView to CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) implementations in existing Xamarin.Forms implementations can be migrated to [`CollectionView`](xref:Xamarin.Forms.CollectionView) implementations with the help of the following table:

| Concept | ListView API | CollectionView |
|---|---|---|
| Data | `ItemsSource` | A [`CollectionView`](xref:Xamarin.Forms.CollectionView) is populated with data by setting its `ItemsSource` property. For more information, see [Populate a CollectionView with data](populate-data.md#populate-a-collectionview-with-data). |
| Item appearance | `ItemTemplate` | The appearance of each item in a [`CollectionView`](xref:Xamarin.Forms.CollectionView) can be defined by setting the `ItemTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). For more information, see [Define item appearance](populate-data.md#define-item-appearance). |
| Cells | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) has no concept of cells. Instead, a data template is used to define the appearance of each item of data in the list. |
| Row separators | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) does not include built-in separators. These can be provided, if desired, in the item template. |
| Selection | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) supports single and multiple selection. For more information, see [Xamarin.Forms CollectionView Selection](selection.md). |
| Row height | `HasUnevenRows`, `RowHeight` | In a `CollectionView`, the row height of each item is determined by the `ItemSizingStrategy` property. For more information, see [Item sizing](layout.md#item-sizing).|
| Caching | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) automatically uses the virtualization provided by the underlying native controls. |
| Headers and footers | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Headers and footers are currently unsupported in `CollectionView`, but will be added in a future release.|
| Grouping | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Grouping is currently unsupported in `CollectionView`, but will be added in a future release. |
| Pull to refresh | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Pull to refresh is currently unsupported in `CollectionView`, but will be added in a future release. |
| Context actions | `ContextActions` | Context actions are currently unsupported in `CollectionView`, but will be added in a future release. |
| Scrolling | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) defines `ScrollTo` methods, that scroll items into view. For more information, see [Scrolling](scrolling.md). |

## Related links

- [CollectionView (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
