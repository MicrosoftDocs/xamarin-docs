---
title: "Programming Language Support in Xamarin"
description: "This document describes various programming languages supported by Xamarin. It discusses C#, F#, portable Visual Basic.NET, and Razor Templates."
ms.service: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: davidortinau
ms.author: daortin
ms.date: 02/18/2018
no-loc: [Objective-C]
---

# Programming Language Support in Xamarin

## C\#

### [Async Support Overview](~/cross-platform/platform/async.md)

Version 5 of C# introduced two new keywords to express asynchronous operations: async and await. These keywords let you write simple code that utilizes the Task Parallel Library to execute long running operations (such as network access) in another thread and easily access the results on completion. The latest versions of Xamarin.iOS and Xamarin.Android support async and await - this document provides explanations and an example of using the new syntax with Xamarin.

### [C# 6 Language Features](~/cross-platform/platform/csharp-six.md)

The latest version of the C# language – version 6 – continues to evolve the language to have less boilerplate, improved clarity, and more consistency. Cleaner initialization syntax, the ability to use `await` in `catch/finally` blocks, and the null-conditional `?` operator are especially useful.

## [F#](fsharp/index.md)

Building mobile apps with F# and Xamarin.

## [Portable Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio supports the creation of Portable Class Libraries using Visual Basic.NET which can then be incorporated into Xamarin applications. This article shows how to create a new Visual Basic PCL and then use it in a sample Xamarin.iOS, Xamarin.Android and Windows Phone application.

## [Building HTML views using Razor Templates](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin enables developers to leverage the Razor templating engine, originally introduced with ASP.NET MVC, along with C# to easily combine data with HTML, Javascript and CSS without the hassle of manually building HTML strings in code.
This article demonstrates how to use Razor templates with Xamarin for Android and iOS.
