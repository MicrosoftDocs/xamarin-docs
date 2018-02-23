---
title: "Advanced Concepts and Internals"
description: "Underlying architecture behind Xamarin.Android and it's API design."
ms.topic: article
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/13/2017
---

# Advanced Concepts and Internals


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

