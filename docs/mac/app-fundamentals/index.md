---
title: "Xamarin.Mac application fundamentals"
description: "This document links to guides that describe various concepts necessary to understand when developing Xamarin.Mac applications."
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/17/2015
---

# Xamarin.Mac application fundamentals

## [Common patterns and idioms](~/mac/app-fundamentals/patterns.md)

Throughout the Apple APIs exposed via C#, certain idioms and patterns come up over and over again. If you have experience with programming with Xamarin.iOS, these may look familiar. Documentation will often refer to these patterns and idioms repeatedly, so having a solid understanding of them will help you make sense of the documentation you find.

## [Understanding Mac APIs](~/mac/app-fundamentals/mac-apis.md)

For much of your time developing with Xamarin.Mac, you can think, read, and write in C# without much concern with the underlying Objective-C APIs. However, sometimes you’ll need to read the API documentation from Apple, translate an answer from Stack Overflow to a solution for your problem, or compare to an existing sample.

## [Console apps](~/mac/app-fundamentals/console.md)

You can also build "headless" console apps that access native macOS APIs using Xamarin.Mac.

## [Working with .xib files](~/mac/app-fundamentals/xib.md)

This article covers working with .xib files created in Xcode's Interface Builder to create and maintain user interfaces for a Xamarin.Mac application.

## [.storyboard/.xib less user interface design](~/mac/app-fundamentals/xibless-ui.md)

This article covers creating a Xamarin.Mac application's user interface directly from C# code without using Xcode's Interface Builder with .storyboard or .xib files.

## [Working with images](~/mac/app-fundamentals/image.md)

This article covers working with images and icons in a Xamarin.Mac application. It covers creating and maintaining the images needed to create your application's icon and using images in both C# code and Xcode's Interface Builder.

## [Data binding and key-value coding](~/mac/app-fundamentals/databinding.md)

This article covers using key-value coding and key-value observing to allow for data binding to UI elements in Xcode's Interface Builder. Using this technique, you greatly reduce the amount of C# code that needs to be written for your Xamarin.Mac application. 

## [Working with databases](~/mac/app-fundamentals/databases.md)

This article covers using key-value coding and key-value observing to allow for data binding with direct access to SQLite databases to UI elements in Xcode's Interface Builder. It also covers using the SQLite.NET ORM to provide access to SQLite data.

## [Working with copy and paste](~/mac/app-fundamentals/copy-paste.md)

This article covers working with the pasteboard to provide copy and paste in a Xamarin.Mac application. It shows how to work with standard data types that can be shared between multiple apps and how to support custom data within a give app.

## [Sandboxing a Xamarin.Mac app](~/mac/app-fundamentals/sandboxing.md)

This article covers sandboxing a Xamarin.Mac application for release on the App Store. It covers all of the elements that go into sandboxing: container directories, entitlements, user-determined permissions, privilege separation, and kernel enforcement.

## [Playing sound with AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

This article shows how to use a helper class to control the playback of sound using an AVAudioPlayer.

## [Reporting bugs](~/mac/app-fundamentals/troubleshooting.md)

Sometimes we all get stuck while working on a project, either on the inability to get an API to work the way we want or in trying to work around a bug. Our goal at Xamarin is for you to be successful in writing your mobile and desktop applications, and we’ve provided some resources to help.
