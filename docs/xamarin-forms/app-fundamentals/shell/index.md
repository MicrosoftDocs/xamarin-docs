---
title: "Xamarin.Forms Shell"
description: "This guide explains how to use Xamarin.Forms Shell, which reduces the complexity of Xamarin.Forms applications by providing the fundamental features that most applications require."
ms.service: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell

## [Introduction](introduction.md)

Xamarin.Forms Shell reduces the complexity of mobile application development by providing the fundamental features that most mobile applications require. This includes a common navigation user experience, a URI-based navigation scheme, and an integrated search handler.

## [Create a Xamarin.Forms Shell application](create.md)

The process for creating a Xamarin.Forms Shell application is to create a XAML file that subclasses the [`Shell`](xref:Xamarin.Forms.Shell) class, set the `MainPage` property of the application's `App` class to the subclassed `Shell` object, and then describe the visual hierarchy of the application in the subclassed `Shell` class.

## [Flyout](flyout.md)

A flyout is the optional root menu for a Shell application, and is accessible through an icon or by swiping from the side of the screen. The flyout consists of an optional header, flyout items, optional menu items, and an optional footer.

## [Tabs](tabs.md)

After a flyout, the next level of navigation in a Shell application is the bottom tab bar. Alternatively, the navigation pattern for an application can begin with bottom tabs and make no use of a flyout. In both cases, when a bottom tab contains more than one page, the pages will be navigable by top tabs.

## [Pages](pages.md)

A [`ShellContent`](xref:Xamarin.Forms.ShellContent) object represents the [`ContentPage`](xref:Xamarin.Forms.ContentPage) object for each [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) or [`Tab`](xref:Xamarin.Forms.Tab).

## [Navigation](navigation.md)

Shell applications can utilize a URI-based navigation scheme that uses routes to navigate to any page in the application, without having to follow a set navigation hierarchy.

## [Search](search.md)

Shell applications can use integrated search functionality that's provided by a search box that can be added to the top of each page.

## [Lifecycle](lifecycle.md)

Shell applications respect the Xamarin.Forms lifecycle, and additionally fire an `Appearing` event when a page is about to appear on the screen, and a `Disappearing` event when a page is about to disappear from the screen.

## [Custom renderers](customrenderers.md)

Shell applications are customizable through the properties and methods that the various Shell classes expose. However, it's also possible to create Shell custom renderers when more sophisticated platform-specific customizations are required.
