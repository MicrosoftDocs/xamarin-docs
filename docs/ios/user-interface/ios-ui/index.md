---
title: "User Interfaces in iOS"
description: "This document links to guides that describe how to build user interfaces in Xamarin.iOS app. The linked guides cover the Appearance API, creating user interface objects, layout options, and more."
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
---

# User Interfaces in iOS

## [Appearance API](introduction-to-the-appearance-api.md)

iOS allows many visual attributes of the user interface controls to be themed using the UIAppearance APIs.

## [Creating User Interface Objects](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple groups related pieces of functionality into “frameworks” which equate to Xamarin.iOS namespaces. `UIKit` is the
namespace that contains all the user interface controls for iOS.

## [Layout Options](~/ios/user-interface/ios-ui/layout-options.md)

There are two different mechanisms for controlling the layout when a view is resized or rotated: Autosizing and Autolayout.

## [Providing Haptic Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

This article covers the new types of haptic feedback available in iOS 10 and how to implement them in Xamarin.iOS.

## [Working with the UI Thread](~/ios/user-interface/ios-ui/ui-thread.md)

Your code should only make changes to user interface controls from the main (or UI) thread. Any UI updates that occur on a different thread (such as a callback or background thread) may not get rendered to the screen, or could even cause a crash.




