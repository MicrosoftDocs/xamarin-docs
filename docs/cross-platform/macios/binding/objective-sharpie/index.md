---
title: "Creating Bindings with Objective Sharpie"
description: "This section provides an introduction to Objective Sharpie, Xamarin's command line tool used to automate the process of creating a binding to an Objective-C Library"
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
---

# Creating Bindings with Objective Sharpie

_This section provides an introduction to Objective Sharpie, Xamarin's command line tool used to automate the process of creating a binding to an Objective-C Library_

- [Overview](#overview) & [History](#history)
- [Getting Started](get-started.md)
- [Tools & Commands](tools.md)
- [Features](platform/index.md)
- [Examples](examples/index.md)
- [Complete Walkthrough](~/ios/platform/binding-objective-c/walkthrough.md)
- [Release History](releases.md)

## Overview

Objective Sharpie is a command line tool to help bootstrap the first pass of a binding.
It works by parsing the header files of a native library to map the public API
into the [binding definition](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (a process that previously was manually done).

Objective Sharpie uses Clang to parse header files, so the binding is as exact and thorough as possible. This can greatly reduce the time and effort it takes to produce a quality binding.

> [!IMPORTANT]
> Objective Sharpie is a tool for experienced Xamarin developers with
> advanced knowledge of Objective-C (and by extension, C). Before
> attempting to bind an Objective-C library you should have solid
> knowledge of how to build the native library on the command line (and a
> good understanding of how the native library works).

## History

We have been evolving and using the Objective Sharpie internally at Xamarin for the last three years. As a testament to the power of Objective Sharpie, APIs introduced in Xamarin.iOS and Xamarin.Mac since iOS 8, Mac OS X 10.10, and watchOS 2.0 were bootstrapped entirely with Objective Sharpie. Xamarin relies heavily on Objective Sharpie internally for building its own products.

However, Objective Sharpie is a very advanced tool that requires advanced knowledge of Objective-C and C, how to use the clang compiler on the command line, and generally how native libraries are put together. Because of this high bar, we felt that having a GUI wizard sets the wrong expectations, and as such, Objective Sharpie is currently only available as a command line tool.

## Related Links

- [Objective Sharpie download](https://aka.ms/objective-sharpie)
- [Walkthrough: Binding an Objective-C Library](~/ios/platform/binding-objective-c/walkthrough.md)
- [Binding Objective-C Libraries](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Binding Details](~/cross-platform/macios/binding/overview.md)
- [Binding Types Reference Guide](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin for Objective-C Developers](~/ios/get-started/objective-c-developers/index.md)
