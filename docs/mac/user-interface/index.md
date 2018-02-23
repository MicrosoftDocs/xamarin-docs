---
title: "User interface"
description: "This article links to guides that describe various macOS UI controls."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
---

# User interface

_This article links to guides that describe various macOS UI controls._

When working with C# and .NET in a Xamarin.Mac application, you have access to the same User Interface controls that a developer working in in *Objective-C* and *Xcode* does. Because Xamarin.Mac integrates directly with Xcode, you can use Xcode's _Interface Builder_ to create and maintain your User Interfaces (or optionally create them directly in C# code). 

The guides listed below give detailed information about working with macOS UI elements in a Xamarin.Mac application. It is highly suggested that you work through the [Hello, Mac](~/mac/get-started/hello-mac.md) article first, specifically the [Introduction to Xcode and Interface Builder](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) and [Outlets and Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, as it covers key concepts and techniques that we'll be using in every article.

You may want to take a look at the [Exposing C# classes / methods to Objective-C](~/mac/internals/how-it-works.md) section of the [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) document as well, it explains the `Register` and `Export` commands used to wire-up your C# classes to Objective-C objects and UI Elements.

## [Windows](~/mac/user-interface/window.md)

This article covers working with Windows and Panels in a Xamarin.Mac application. It covers creating and maintaining Windows and Panels in Xcode and Interface builder, loading Windows and Panels from `.storyboard` or `.xib` files, using Windows and responding to Windows in C# code.

## [Dialogs](~/mac/user-interface/dialog.md)

This article covers working with Dialogs and Modal Windows in a Xamarin.Mac application. It covers creating and maintaining Modal Windows in Xcode and Interface builder, working with standard dialogs, displaying and responding to Windows in C# code.

## [Alerts](~/mac/user-interface/alert.md)

This article covers working with Alerts in a Xamarin.Mac application. It covers creating and displaying Alerts from C# code and responding to Alerts.

## [Menus](~/mac/user-interface/menu.md)

Menus are used in various parts of a Mac application's user interface; from the application's main menu at the top of the screen to pop-up and contextual menus that can appear anywhere in a window. Menus are a integral part of a Mac application's user experience. This article covers working with Cocoa Menus in a Xamarin.Mac application.

## [Standard Controls](~/mac/user-interface/standard-controls.md)

Working with the standard AppKit controls such as Buttons, Labels, Text Fields, Check Boxes and Segmented Controls in a Xamarin.Mac application. This guide covers adding them to a User Interface Design in Xcode's Interface Builder, exposing them to code through Outlets and Actions and working with AppKit Controls in C# Code.

 
## [Toolbars](~/mac/user-interface/toolbar.md)

This article covers working with Toolbars in a Xamarin.Mac application. It covers creating and maintaining Toolbars in Xcode and Interface builder, how to expose the Toolbar Items to code using Outlets and Actions, enabling and disabling Toolbar Items and finally responding to Toolbar Items in C# code.

## [Table Views](~/mac/user-interface/table-view.md)

This article covers working with Table Views in a Xamarin.Mac application. It covers creating and maintaining Table Views in Xcode and Interface builder, how to expose the Table View Items to code using Outlets and Actions, populating Table Views and finally responding to Table View Items in C# code.

## [Outline Views](~/mac/user-interface/outline-view.md)

This article covers working with Outline Views in a Xamarin.Mac application. It covers creating and maintaining Outline Views in Xcode and Interface builder, how to expose the Outline View Items to code using Outlets and Actions, populating Outline Items and finally responding to Outline View Items in C# code.

## [Source Lists](~/mac/user-interface/source-list.md)

This article covers working with Source Lists in a Xamarin.Mac application. It covers creating and maintaining Source Lists in Xcode and Interface builder, how to expose the Source Lists Items to code using Outlets and Actions, populating Source List Items and finally responding to Source List Items in C# code.

## [Collection Views](~/mac/user-interface/collection-view.md)

This article covers working with Collection Views in a Xamarin.Mac application. It covers creating and maintaining Collection Views in Xcode and Interface builder, how to expose the Collection View elements to code using Outlets and Actions, populating Collection Views and finally responding to Collection Views in C# code.

## [Creating Custom User Controls](~/mac/user-interface/custom-controls.md)

This article covers creating custom User Interface controls (by inheriting from `NSControl`), drawing a custom interface for the control and creating custom actions that can be used with Xcode's Interface builder.

## Mac Samples Gallery

We also suggest taking a look at the [Mac Samples Gallery](http://developer.xamarin.com/samples/mac/all/), it includes a wealth of ready-to-use code that can help you get a Xamarin.Mac project off the ground quickly.

## Related Links

- [macOS Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
