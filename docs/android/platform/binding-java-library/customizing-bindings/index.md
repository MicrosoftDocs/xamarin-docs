---
title: "Customizing Bindings"
description: "You can customize an Xamarin.Android binding by editing the metadata that controls the binding process. These manual modifications are often necessary for resolving build errors and for shaping the resulting API so that it is more consistent with C#/.NET. These guides explain the structure of this metadata, how to modify the metadata, and how to use JavaDoc to recover the names of method parameters."
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
---

# Customizing Bindings

_You can customize an Xamarin.Android binding by editing the metadata that controls the binding process. These manual modifications are often necessary for resolving build errors and for shaping the resulting API so that it is more consistent with C#/.NET. These guides explain the structure of this metadata, how to modify the metadata, and how to use JavaDoc to recover the names of method parameters._


## Overview
 
Xamarin.Android automates much of the binding process; however, in some
cases manual modification is required to address the following issues:

-   Resolving build errors caused by missing types, obfuscated types, 
    duplicate names, class visibility issues, and other situations that 
    cannot be resolved by the Xamarin.Android tooling. 

-   Changing the mapping that Xamarin.Android uses to bind the Android 
    API to different types in C# (for example, many developers prefer to map
    Java `int` constants to C# `enum` constants).

-   Removing unused types that do not need to be bound. 

-   Adding types that have no counterpart in the underlying Java API. 

You can make some or all of these changes by modifying the metadata
that controls the binding process.


## Guides

The following guides describe the metadata that controls the binding process and 
explain how to modify this metadata to address these issues:

-   [Java Bindings Metadata](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)
    provides an overview of the metadata that goes into a Java binding.
    It describes the various manual steps that are sometimes required to
    complete a Java binding library, and it explains how to shape an API
    exposed by a binding to more closely follow .NET design guidelines.

-   [Naming Parameters with Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)
    explains how to recover parameter names in a Java Binding Project by using Javadoc produced from the bound Java project.


 

