---
title: "Navigation"
description: "Xamarin.Forms provides a number of different page navigation experiences, depending upon the Page type being used."
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
---

# Navigation

_Xamarin.Forms provides a number of different page navigation experiences, depending upon the Page type being used._

![](images/page-types.png "Xamarin.Forms Page Types")

## [Hierarchical Navigation](hierarchical.md)

The [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) class provides a hierarchical navigation experience where the user is able to navigate through pages, forwards and backwards, as desired. The class implements navigation as a last-in, first-out (LIFO) stack of [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objects.

## [TabbedPage](tabbed-page.md)

The Xamarin.Forms [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) consists of a list of tabs and a larger detail area, with each tab loading content into the detail area.

## [CarouselPage](carousel-page.md)

The Xamarin.Forms [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) is a page that users can swipe from side to side to navigate through pages of content, like a gallery.

## [MasterDetailPage](master-detail-page.md)

The Xamarin.Forms [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) is a page that manages two pages of related information – a master page that presents items, and a detail page that presents details about items on the master page.

## [Modal Pages](modal.md)

Xamarin.Forms also provides support for modal pages. A modal page encourages users to complete a self-contained task that cannot be navigated away from until the task is completed or cancelled.

## [Displaying Pop-Ups](pop-ups.md)

Xamarin.Forms provides two pop-up-like user interface elements: an alert and an action sheet. These interface elements can be used to ask users simple questions and to guide users through tasks.

