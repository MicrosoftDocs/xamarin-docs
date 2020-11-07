---
title: "Xamarin.Forms Shell Lifecycle"
description: "Shell applications respect the Xamarin.Forms lifecycle, and an Appearing event is raised when a page is about to appear on the screen, and a Disappearing event is raised when a page is about to disappear from the screen."
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell Lifecycle

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Shell applications respect the Xamarin.Forms lifecycle, and an `Appearing` event is raised when a page is about to appear on the screen, and a `Disappearing` event is raised when a page is about to disappear from the screen. These events are propagated to pages, and can be handled by overriding the [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) or [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) methods on the page.

> [!NOTE]
> In a Shell application, the `Appearing` and `Disappearing` events are raised from cross-platform code, prior to platform code making a page visible, or removing a page from the screen.

For more information about the Xamarin.Forms app lifecycle, see [Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

## Hierarchical navigation

In a Shell application, pushing a page onto the navigation stack will result in the currently visible `ShellContent` object, and its page content, raising the `Disappearing` event. Similarly, popping the last page from the navigation stack will result in the newly visible `ShellContent` object, and its page content, raising the `Appearing` event.

For more information about hierarchical navigation, see [Xamarin.Forms Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## Modal navigation

In a Shell application, pushing a modal page onto the modal navigation stack will result in all visible Shell objects raising the `Disappearing` event. Similarly, popping the last modal page from the modal navigation stack will result in all visible Shell objects raising the `Appearing` event.

For more information about modal navigation, see [Xamarin.Forms Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md)