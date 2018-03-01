---
title: "Xamarin.Forms Pages"
description: "Xamarin.Forms Pages represent cross-platform mobile app screens."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
---

# Xamarin.Forms Pages

_Xamarin.Forms Pages represent cross-platform mobile app screens._

## Pages

The [`Page`](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) class is a visual element that occupies most or all of the screen and contains a single child. A `Page` object represents a `View Controller` in iOS or a `Page` in Windows Phone. On Android each page takes up the screen like an `Activity`, but Xamarin.Forms pages are *not* `Activity` objects.

 [ ![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png "Xamarin.Forms Page Types")

Xamarin.Forms supports:

| Type | Description | Screenshot |
| ---- | ----------- | ---------- |
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | A [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) displays a single [`View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), often a container such as a [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) or a [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/). | [![ContentPage Example](pages-images/ContentPage.png "ContentPage Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) |
| [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) | A [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) that manages two panes of information. | [![MasterDetailPage Example](pages-images/MasterDetailPage.png "MasterDetailPage Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) |
| [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) | A [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) that manages the navigation and user-experience of a stack of other pages.  | [![NavigationPage Example](pages-images/NavigationPage.png "NavigationPage Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) |
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) | A [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) that allows navigation between children pages, using tabs. | [![TabbedPage Example](pages-images/TabbedPage.png "TabbedPage Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) |
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) | A [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) that displays full-screen content with a control template, and the base class for [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). | [![TemplatedPage Example](pages-images/TemplatedPage.png "TemplatedPage Example")](https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates) |
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) | A [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) allowing swipe gestures between subpages, like a gallery. | [![CarouselPage Example](pages-images/CarouselPage.png "CarouselPage Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) |

## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery sample](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API Documentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
