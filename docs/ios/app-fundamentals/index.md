---
title: "Xamarin.iOS application fundamentals"
description: "This document links to various guides that describe concepts foundational to Xamarin.iOS development, such as app transport security, backgrounding, events, and threading."
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/21/2017
---
# Xamarin.iOS application fundamentals

This section provides a guide on some of the more common things tasks or
concepts that developers need to be aware of when developing Xamarin.iOS (formerly MonoTouch) applications.

## [Accessibility](~/ios/app-fundamentals/accessibility.md)

This document describes various APIs and tools that can be used to help
build applications that are accessible to as many users as possible.

## [App Transport Security](~/ios/app-fundamentals/ats.md)

This article will introduce the security changes that App Transport Security enforces on an iOS 9 app and what this means for your Xamarin.iOS projects, it will cover the ATS configuration options and it will cover how to opt-out of ATS, if required. Because ATS is enabled by default, any non-secure internet connections will raise an exception in iOS 9 apps (unless you've explicitly allowed it).

## [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)

Background processing or backgrounding is the process of letting applications perform tasks in the background while another application is running in the foreground. This guide serves as an introduction to background processing in iOS.

## [Creating iOS applications in code](~/ios/app-fundamentals/ios-code-only.md)

This article examines how to create iOS applications entirely in code using Visual Studio and Visual Studio for Mac. It shows how to start from an empty project template to build an application screen in a controller by creating a hierarchy of views from UIKit. Then, it discusses how to create custom views that can be loaded in a controller.

## [Events, protocols, and delegates](~/ios/app-fundamentals/delegates-protocols-and-events.md)

This article presents the key iOS technologies used to receive callbacks and to populate user interface controls with data. These technologies are events, protocols, and delegates; this article explains what each of these is and how each is used from C#. It demonstrates how Xamarin.iOS uses iOS controls to expose familiar .NET events, as well as how Xamarin.iOS provides support for Objective-C concepts such as protocols and delegates (Objective-C delegates should not be confused with C# delegates). This article also provides examples that show how protocols are used both as the basis for Objective-C delegates and in non-delegate scenarios.

## [Working with the file system](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS can use the same System.IO classes to work with files and
directories in iOS that you would use in any .NET application. However, despite
the familiar classes and methods, iOS implements some restrictions on the files
that can be created or accessed and also provides special features for certain
directories. This article outlines these restrictions and features, and
demonstrates how file access works in a Xamarin.iOS application.

## [Working with images](~/ios/app-fundamentals/images-icons/index.md)

This article examines how to use images in Xamarin.iOS, both application
support images (such as icons, loading images, etc.) and images within
applications (such as images applied to controls). It also covers how to use
Visual Studio for Mac to incorporate images as well as how to interact with images from
code.

## [Localization](~/ios/app-fundamentals/localization/index.md)

This guide covers the addition of encodings to a Xamarin.iOS application to
support internationalization.

## [Working with property lists](~/ios/app-fundamentals/index.md)

This document introduces Visual Studio for Mac's graphical and advanced property list (.plist) editor for working with Info.plist and Entitlements.plist. It illustrates setting icons and launch images for iOS application, and demonstrates specifying app capabilities (entitlements) from inside Visual Studio for Mac.

## [Working with security and privacy](~/ios/app-fundamentals/security-privacy.md)

Apple has made several enhancements to both security and privacy in iOS 10 (and greater) that will help the developer improve the security of their apps and ensure the end user's privacy. This article will cover implementing these features in a Xamarin.iOS app.

## [Threading](~/ios/app-fundamentals/threading.md)

This article discusses threading in a Xamarin.iOS application, and talks a bit
about the .NET thread pool, responsive applications, and garbage
collection.

## [Touch](~/ios/app-fundamentals/touch/index.md)

Touch screens on many of today’s devices allow users to quickly and efficiently interact with devices in a natural and intuitive way. This interaction is not limited just to simple touch detection – it is possible to use gestures as well. For example, the pinch-to-zoom gesture is a very common example of this – by pinching a part of the screen with two fingers the user can zoom in or out. This guide examines touch and gestures in iOS.

## [Working with user defaults](~/ios/app-fundamentals/user-defaults.md)

The `NSUserDefaults` class provides a way for iOS Apps and Extensions to programmatically interact with the system-wide Default System. By using the Defaults System, the user can configure an app's behavior or styling to meet their preferences (based on the design of the app). For example, to present data in Metric vs Imperial measurements or select a given UI Theme.
