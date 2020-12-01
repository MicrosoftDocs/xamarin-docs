---
title: "Building User Interfaces with Xamarin.iOS"
description: "This document describes how to build a user interface in a Xamarin.iOS app. It provides links to guides about the iOS designer, storyboards, general iOS interface concepts, and iOS user interface controls."
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/21/2017
---

# Building User Interfaces with Xamarin.iOS

## [Storyboards](~/ios/user-interface/storyboards/index.md)

A Storyboard is a visual representation of the appearance and flow of your application. Visual Studio for Mac allows you to interact with the Xcode Interface Builder to design your application screen visually as well as access the views, controllers, and segues with C# for more control. 

## [iOS Designer](~/ios/user-interface/designer/index.md)

> [!WARNING]
> The iOS Designer will start to be phased out in Visual Studio 2019 version 16.8 and Visual Studio 2019 for Mac version 8.8.
> The recommended way to build iOS user interfaces is directly on a Mac running Xcode. For more information, see [Designing user interfaces with Xcode](~/ios/user-interface/storyboards/index.md). 

We have built a designer for the iOS storyboard format which is fully integrated
into Visual Studio for Mac. The iOS designer maintains full compatibility with the storyboard format, so that files can be edited in either Xcode or Visual Studio for Mac. Additionally, the editor supports advanced features, such as custom controls that render at design-time in the editor.

## [User Interface in iOS](~/ios/user-interface/ios-ui/index.md)

Covers working with the iOS User Interface in a Xamarin.iOS app including: the Appearance API, Creating User Interface Objects, Layout Options, Providing Haptic Feedback and Working with the UI Thread.

## [User Interface Controls](~/ios/user-interface/controls/index.md)

Xamarin.iOS exposes all the native user interface objects provided by Apple. They are easily added to Xamarin.iOS applications using the iOS Designer, Xcode's Interface Builder or programmatically. Regardless of which method you choose, Xamarin.iOS exposes all the user interface object properties and methods in C#.
