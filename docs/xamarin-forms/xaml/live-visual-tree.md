---
title: "Xamarin.Forms Live Visual Tree"
description: See the runtime UI hierarchy of your app while debugging.
ms.service: xamarin
ms.assetid: 29c45b85-21b1-40ab-941f-4d9e57770c46
ms.subservice: xamarin-forms
author: BretJohnson
ms.author: bretjohn
ms.date: 03/25/2021
no-loc: [Xamarin.Forms]
---

# Xamarin.Forms live visual tree

You can receive a real-time view of your running XAML code with the **Live Visual Tree**. It shows a tree view of the UI elements of your running Xamarin.Forms application.

## Requirements

* Use Xamarin.Forms 5.0 or newer.
* Have *changes only* Hot Reload enabled (it's enabled by default).

## Usage

With the requirements met, debug your app and Live Visual Tree window will show the runtime UI hierarchy of your app.

  * **Windows**: By default, it appears on the IDE's left. If you don't see it, use **Debug > Windows > Live Visual Tree** to show it.
  * **Mac**: By default, it appears on the IDE's right. If you don't see it, use **View > Debug Windows > Live Visual Tree** to show it.

Use the tree view to inspect the runtime UI hierarchy for your app, expanding/collapsing nodes to focus on particular parts of the UI.

## Live visual tree toolbar 

The view of XAML elements is simplified by default using the **Just My XAML** feature. Toggle the **Show Just My XAML** button, rightmost on the Live Visual Tree toolbar, to show all UI elements. If you wish you can [disable this setting](/visualstudio/debugger/general-debugging-options-dialog-box) in options to always show all XAML elements.

> [!NOTE]
> Visual Studio for Mac doesn't currently support the **Just My XAML** feature.

The structure of the XAML has a lot of elements that you're probably not directly interested in, and if you don't know the code well you might have a hard time navigating the tree to find what you're looking for. Therefore, the **Live Visual Tree** has multiple approaches that let you use the application's UI to help you find the element you want to examine.

**Select element in the running application** (currently only supported for UWP apps). You can enable this mode when you select the leftmost button on the **Live Visual Tree** toolbar. With this mode on, you can select a UI element in the application, and the **Live Visual Tree** automatically updates to show the node in the tree corresponding to that element, and its properties. 

**Display layout adorners in the running application** (currently only supported for UWP apps). You can enable this mode when you select the button that is immediately to the right of the Enable selection button. When **Display layout adorners** is on, it causes the application window to show horizontal and vertical lines along the bounds of the selected object so you can see what it aligns to, as well as rectangles showing the margins.

**Preview Selection**. You can enable this mode by selecting the third button from the left on the Live Visual Tree toolbar. This mode shows the XAML where the element was declared, if you have access to the source code of the application.

## Related links

- [Write and debug running XAML code with XAML Hot Reload](hot-reload.md)
