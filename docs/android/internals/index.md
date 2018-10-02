---
title: "Advanced Concepts and Internals"
description: "Underlying architecture behind Xamarin.Android and it's API design."
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
---

# Advanced Concepts and Internals

_This section contains topics that explain the architecture, API
design, and limitations of Xamarin.Android. In addition, it includes
topics that explain its garbage collection implementation and the
assemblies that are available in Xamarin.Android. Because
Xamarin.Android is
[open-source](https://github.com/xamarin/xamarin-android), it is also
possible to understand the inner workings of Xamarin.Android by
examining its source code._


##  [Architecture](~/android/internals/architecture.md)

This article explains the underlying architecture behind a 
Xamarin.Android application. It explains how Xamarin.Android 
applications run inside a Mono execution environment alongside with the 
Android runtime Virtual Machine and explains such key concepts as Android 
Callable Wrappers and Managed Callable Wrappers. 



##  [API Design](~/android/internals/api-design.md)

In addition to the core Base Class Libraries that are part of Mono,
Xamarin.Android ships with bindings for various Android APIs to allow
developers to create native Android applications with Mono.

At the core of Xamarin.Android there is an interop engine that bridges the
C# world with the Java world and provides developers with access to the Java
APIs from C# or other .NET languages.



##  [Assemblies](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android ships with several assemblies. Just as Silverlight is 
an extended subset of the desktop .NET assemblies, Xamarin.Android is 
also an extended subset of several Silverlight and desktop .NET 
assemblies. 

