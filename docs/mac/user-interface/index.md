---
title: "macOS user interface controls in Xamarin.Mac"
description: "This document links to guides that describe various user interface controls available to Xamarin.Mac developers. Linked content takes a look at windows, dialogs, alerts, menus, toolbars, table views, outline views, and more."
ms.service: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
no-loc: [Objective-C]
---

# macOS user interface controls in Xamarin.Mac

_This article links to guides that describe various macOS UI controls._

When working with C# and .NET in a Xamarin.Mac application, you have access to the same user interface controls that a developer working in *Objective-C* and *Xcode* does. Because Xamarin.Mac integrates directly with Xcode, you can use Xcode's _Interface Builder_ to create and maintain your user interfaces (or optionally create them directly in C# code).

The guides listed below give detailed information about working with macOS UI elements in a Xamarin.Mac application. It is highly suggested that you work through the [Hello, Mac](~/mac/get-started/hello-mac.md) article first, specifically the [Introduction to Xcode and Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) and [Outlets and Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, as it covers key concepts and techniques that we'll be using in every article.

You may want to take a look at the [Exposing C# classes / methods to Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) section of the [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) document as well, as it explains the `Register` and `Export` attributes used to wire-up your C# classes to Objective-C objects and UI elements.

## [Windows](~/mac/user-interface/window.md)

This article covers working with windows and panels in a Xamarin.Mac application. It covers creating and maintaining windows and panels in Xcode and Interface Builder, loading windows and panels from .storyboard or .xib files, using windows, and responding to windows in C# code.

## [Dialogs](~/mac/user-interface/dialog.md)

This article covers working with dialogs and modal windows in a Xamarin.Mac application. It covers creating and maintaining modal windows in Xcode and Interface Builder, working with standard dialogs, and displaying and responding to windows in C# code.

## [Alerts](~/mac/user-interface/alert.md)

This article covers working with alerts in a Xamarin.Mac application. It covers creating and displaying alerts from C# code and responding to alerts.

## [Menus](~/mac/user-interface/menu.md)

Menus are used in various parts of a Mac application's user interface; from the application's main menu at the top of the screen to pop-up menus and contextual menus that can appear anywhere in a window. Menus are an integral part of a Mac application's user experience. This article covers working with Cocoa menus in a Xamarin.Mac application.

## [Standard controls](~/mac/user-interface/standard-controls.md)

Working with the standard AppKit controls such as buttons, labels, text fields, check boxes, and segmented controls in a Xamarin.Mac application. This guide covers adding them to a user interface design in Xcode's Interface Builder, exposing them to code through outlets and actions, and working with AppKit controls in C# code.

## [Toolbars](~/mac/user-interface/toolbar.md)

This article covers working with toolbars in a Xamarin.Mac application. It covers creating and maintaining toolbars in Xcode and Interface Builder, how to expose the toolbar items to code using outlets and actions, enabling and disabling toolbar items, and finally responding to Toolbar items in C# code.

## [Table views](~/mac/user-interface/table-view.md)

This article covers working with table views in a Xamarin.Mac application. It covers creating and maintaining table views in Xcode and Interface Builder, how to expose the table view items to code using outlets and actions, populating table views, and responding to table view items in C# code.

## [Outline views](~/mac/user-interface/outline-view.md)

This article covers working with outline views in a Xamarin.Mac application. It covers creating and maintaining outline views in Xcode and Interface Builder, how to expose the outline view items to code using outlets and actions, populating outline views, and responding to outline view items in C# code.

## [Source lists](~/mac/user-interface/source-list.md)

This article covers working with source lists in a Xamarin.Mac application. It covers creating and maintaining source lists in Xcode and Interface Builder, how to expose source list items to code using outlets and actions, populating source lists, and responding to source list items in C# code.

## [Collection views](~/mac/user-interface/collection-view.md)

This article covers working with collection views in a Xamarin.Mac application. It covers creating and maintaining collection views in Xcode and Interface Builder, how to expose the collection view items to code using outlets and actions, populating collection views, and responding to collection views in C# code.

## [Creating custom controls](~/mac/user-interface/custom-controls.md)

This article covers creating custom user interface controls (by inheriting from `NSControl`), drawing a custom interface for the control, and creating custom actions that can be used with Xcode's Interface Builder.

## Mac samples gallery

We also suggest taking a look at the [Mac Samples Gallery](/samples/browse/?products=xamarin&term=Xamarin.Mac). It includes a wealth of ready-to-use code that can help you get a Xamarin.Mac project off the ground quickly.

## Related links

- [macOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/designing-for-macos)