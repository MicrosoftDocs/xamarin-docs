---
title: "Xamarin.Forms Shell lifecycle"
description: "Shell applications respect the Xamarin.Forms lifecycle, and additionally fire an Appearing event when a page is about to appear on the screen, and a Disappearing event when a page is about to disappear from the screen."
ms.service: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/15/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell lifecycle

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Shell applications respect the Xamarin.Forms lifecycle, and additionally fire an [`Appearing`](xref:Xamarin.Forms.BaseShellItem.Appearing) event when a page is about to appear on the screen, and a [`Disappearing`](xref:Xamarin.Forms.BaseShellItem.Disappearing) event when a page is about to disappear from the screen. These events are propagated to pages, and can be handled by overriding the [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) or [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) methods on the page.

> [!NOTE]
> In a Shell application, the `Appearing` and `Disappearing` events are raised from cross-platform code, prior to platform code making a page visible, or removing a page from the screen.

For more information about the Xamarin.Forms app lifecycle, see [Xamarin.Forms app lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

## Hierarchical navigation

In a Shell application, pushing a page onto the navigation stack will result in the currently visible [`ShellContent`](xref:Xamarin.Forms.ShellContent) object, and its page content, raising the [`Disappearing`](xref:Xamarin.Forms.BaseShellItem.Disappearing) event. Similarly, popping the last page from the navigation stack will result in the newly visible `ShellContent` object, and its page content, raising the  [`Appearing`](xref:Xamarin.Forms.BaseShellItem.Appearing) event.

For more information about hierarchical navigation, see [Xamarin.Forms hierarchical navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## Modal navigation

In a Shell application, pushing a modal page onto the modal navigation stack will result in all visible Shell objects raising the [`Disappearing`](xref:Xamarin.Forms.BaseShellItem.Disappearing) event. Similarly, popping the last modal page from the modal navigation stack will result in all visible Shell objects raising the [`Appearing`](xref:Xamarin.Forms.BaseShellItem.Appearing) event.

For more information about modal navigation, see [Xamarin.Forms modal pages](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms app lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms modal pages](~/xamarin-forms/app-fundamentals/navigation/modal.md)
