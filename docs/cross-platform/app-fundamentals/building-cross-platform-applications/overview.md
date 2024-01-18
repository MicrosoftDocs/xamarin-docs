---
title: "Building Cross Platform Applications Overview"
description: "This document provides a high-level overview of building cross-platform applications. It discusses the value of C#, design patterns such as MVC/MVVM, and native UIs."
ms.service: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
no-loc: [Objective-C]
---

# Building Cross Platform Applications Overview

This guide first introduces the Xamarin platform. It will discuss how to architect a
cross-platform application to maximize code reuse. Then finally, how to deliver a high-quality
native experience on iOS and Android mobile platforms.

The approach used in this document can be used for both productivity apps and game apps, however the focus is on productivity and utility 
(non-game applications). See [Visual Studio Tools for Unity](/visualstudio/cross-platform/visual-studio-tools-for-unity) for
cross-platform game development guidance.

The phrase “write-once, run everywhere” is often used to extol the
virtues of a single codebase that runs unmodified on multiple platforms. While
it has the benefit of code reuse, that approach has drawbacks. Two common drawbacks are applications
that have a lowest-common-denominator feature-set, and a generic-looking user
interface that doesn't fit nicely into any of the target platforms.

Xamarin isn't just a “write-once, run everywhere” platform, because one
of its strengths is the ability to implement native user interfaces specifically
for each platform. However, with thoughtful design it’s still possible to
share most of the non-user interface code and get the best of both worlds. Write
your data storage and business logic code once, and present native UIs on each
platform. This document discusses a general architectural approach to achieve
this goal.

Here's a summary of the key points for creating Xamarin cross-platform
apps:

- **Use C#** - Write your apps in C#. Existing code written in C# can be ported to iOS and Android using Xamarin easily, and used in Windows apps.
- **Utilize MVC or MVVM design patterns** - Develop your application’s User Interface using the Model/View/Controller pattern. Architect your application using a Model/View/Controller approach or a Model/View/ViewModel approach where there is a clear separation between the “Model” and the rest. Determine which parts of your application will be using native user interface elements of each platform (iOS, Android, Windows, Mac) and use this as a guideline to split your application into two components: “Core” and “User-Interface”.
- **Build native UIs** - Each OS-specific application provides a different user-interface layer (implemented in C# with the assistance of native UI design tools):

1. On iOS, use the UIKit APIs to create native-looking applications, using Storyboards for the presentation layer, created in Xcode.
1. On Android, use Android.Views to create native-looking applications, taking advantage of Xamarin’s UI designer.
1. On Windows you'll be using XAML for presentation layer, created in Visual Studio or Blend’s UI designer.
1. On Mac, you'll use Storyboards for the presentation layer, created in Xcode.

Xamarin.Forms projects are supported on all platforms, and allow you create user interfaces that can be shared across platforms using Xamarin.Forms XAML.

The amount of code reuse will depend largely on how much code is kept in the
shared core and how much code is user-interface specific. The core code is
anything that does not interact directly with the user, but instead provides
services for parts of the application that will collect and display this
information.

To increase the amount of code reuse, you can adopt cross-platform
components that provide common services across all these systems such as:

1. [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) for local SQL storage,
1. [Xamarin Plugins](https://xamarin.com/plugins) for accessing device-specific capabilities including the camera, contacts and geolocation,
1. [NuGet packages](https://nuget.org) that are compatible with Xamarin projects, such as [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1. Using .NET framework features for networking, web services, IO and more.

Some of these components are implemented in the *Tasky* case study.

 <a name="Separate_Reusable_Code_into_a_Core_Library"></a>

## Separate Reusable Code into a Core Library

By following the principle of separation of responsibility by layering your application architecture and then moving core functionality that is platform agnostic into a reusable core library, you can maximize code sharing across platforms, as the figure below illustrates:

 ![By following the principle of separation of responsibility by layering your application architecture and then moving core functionality that is platform agnostic into a reusable core library, you can maximize code sharing across platforms](overview-images/layers2.png)

 <a name="Case_Studies"></a>

## Case Studies

There is one case study that accompanies this document – *Tasky Pro*. Each case study discusses the
implementation of the concepts outlined in this document in a real-world
example. The code is open source and available on [github](https://github.com/xamarin/mobile-samples/).
