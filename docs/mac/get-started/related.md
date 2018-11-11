---
title: "Xamarin.Mac – Related Documentation"
description: "This document provide links to documentation relevant for Xamarin.Mac developers: Xamarin.iOS documentation, Apple's Mac Dev Center, and various guides that describe how to build user interfaces with Xamarin.Mac."
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/02/2016
---

# Xamarin.Mac – Related Documentation

In addition to the Mac section of [developer.xamarin.com](~/mac/get-started/index.md) there are three great sources of documentation that can also be of assistance with Xamarin.Mac questions:

- [**Xamarin.iOS documentation**](~/ios/get-started/index.md) - For many APIs (primarily outside of AppKit/UIKit) there are only small differences between the iOS and macOS versions. In some cases where a given iOS API has a name of `UIFoo`, a similar API named `NSFoo` can be found on macOS. These examples will be generally in C# already.

- **Apple’s [Mac Dev Center](https://developer.apple.com/devcenter/mac/)** -  Many times an example of what APIs to call in Objective-C can be converted to C# in a straightforward manner. See [Understanding Mac APIs](~/mac/app-fundamentals/mac-apis.md) for details on how to do this.

- [**Stack Overflow**](http://stackoverflow.com/) - A great resource for simple one off questions such as ["How do I auto expand all nodes in an NSOutlineView"](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). These examples will often be in Objective-C and need to be converted to C#, but there is a subset of answers in C#.

## User Interface

When working with C# and .NET in a Xamarin.Mac application, the Developer has access to the same User Interface controls that a developer working in *Objective-C* and *Xcode* does. Because Xamarin.Mac integrates directly with Xcode, the developer can use Xcode's _Interface Builder_ to create and maintain an app's User Interfaces (or optionally create them directly in C# code).

The guides listed below give detailed information about working with macOS elements in a Xamarin.Mac application:

- [Windows](~/mac/user-interface/window.md)
- [Dialogs](~/mac/user-interface/dialog.md)
- [Alerts](~/mac/user-interface/alert.md)
- [Menus](~/mac/user-interface/menu.md)
- [Toolbars](~/mac/user-interface/toolbar.md)
- [Table Views](~/mac/user-interface/table-view.md)
- [Outline Views](~/mac/user-interface/outline-view.md)
- [Source Lists](~/mac/user-interface/source-list.md)
