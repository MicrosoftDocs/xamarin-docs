---
title: "Custom Renderers"
description: "Xamarin.Forms user interfaces are rendered using the native controls of the target platform, allowing Xamarin.Forms applications to retain the appropriate look and feel for each platform. Custom Renderers let developers override this process to customize the appearance and behavior of Xamarin.Forms controls on each platform."
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
---

# Custom Renderers

_Xamarin.Forms user interfaces are rendered using the native controls of the target platform, allowing Xamarin.Forms applications to retain the appropriate look and feel for each platform. Custom Renderers let developers override this process to customize the appearance and behavior of Xamarin.Forms controls on each platform._

## [Introduction to custom renderers](introduction.md)

Custom renderers provide a powerful approach for customizing the appearance and behavior of Xamarin.Forms controls. They can be used for small styling changes or sophisticated platform-specific layout and behavior customization. This article provides an introduction to custom renderers, and outlines the process for creating a custom renderer.

## [Renderer base classes and native controls](renderers.md)

Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell.

## [Customizing an Entry](entry.md)

The Xamarin.Forms [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) control allows a single line of text to be edited. This article demonstrates how to create a custom renderer for the `Entry` control, enabling developers to override the default native rendering with their own platform-specific customization.

## [Customizing a ContentPage](contentpage.md)

A [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) is a visual element that displays a single view and occupies most of the screen. This article demonstrates how to create a custom renderer for the `ContentPage` page, enabling developers to override the default native rendering with their own platform-specific customization.

## [Customizing a Map](map/index.md)

Xamarin.Forms.Maps provides a cross-platform abstraction for displaying maps that use the native map APIs on each platform, to provide a fast and familiar map experience for users. This topic demonstrates how to create custom renderers for the `Map` control, enabling developers to override the default native rendering with their own platform-specific customization.

## [Customizing a ListView](listview.md)

A Xamarin.Forms [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) is a view that displays a collection of data as a vertical list. This article demonstrates how to create a custom renderer that encapsulates platform-specific list controls and native cell layouts, allowing more control over native list control performance.

## [Customizing a ViewCell](viewcell.md)

A Xamarin.Forms [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) is a cell that can be added to a [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) or [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), which contains a developer-defined view. This article demonstrates how to create a custom renderer for a `ViewCell` that's hosted inside a Xamarin.Forms `ListView` control. This stops the Xamarin.Forms layout calculations from being repeatedly called during `ListView` scrolling.

## [Implementing a View](view.md)

Xamarin.Forms custom user interfaces controls should derive from the [`View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) class, which is used to place layouts and controls on the screen. This article demonstrates how to create a custom renderer for a Xamarin.Forms custom control that's used to display a preview video stream from the device's camera.

## [Implementing a HybridWebView](hybridwebview.md)

This article demonstrates how to create a custom renderer for a `HybridWebView` custom control, which demonstrates how to enhance the platform-specific web controls to allow C# code to be invoked from JavaScript.

## [Implementing a video player](video-player/index.md)

This article shows how to write renderers to implement a custom `VideoPlayer` control that can play videos from the web, videos embedded as application resources, or videos stored in the video library on the user's device. Several techniques are demonstrated, including implementing methods and read-only bindable properties. 


## Related Links

- [Effects](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Custom Renderers (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Custom Renderers (Xamarin University Video) Sample](http://bit.ly/xf-customrenderer)
