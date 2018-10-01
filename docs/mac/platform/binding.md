---
title: "Binding Mac libraries for Xamarin.Mac"
description: "This document links to guides that describe how to work with Objective-C bindings in a Xamarin.Mac application, including Objective Sharpie and sample code."
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 01/13/2017
---

# Binding Mac libraries for Xamarin.Mac

Follow these links to learn about binding Objective-C libraries
on Xamarin.Mac:

- [**Overview**](~/cross-platform/macios/binding/overview.md) -
  Describes how binding works.
- [**Binding Objective-C Libraries**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Instructions on how to bind Objective-C libraries for use in Xamarin projects.
- [**Type Definition Reference Guide**](~/cross-platform/macios/binding/binding-types-reference.md) -
  Describes all of the attributes available to binding authors to drive the binding
  generation process.

## [Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objective Sharpie is a command-line tool to help bootstrap the first pass of a binding.
It works by parsing the header files of a native library to map the public API
into the [binding definition](~/cross-platform/macios/binding/binding-types-reference.md)
(a process that is otherwise done manually). Objective Sharpie does not create
a binding by itself, but it can help get you started!

## Examples

Refer to the [XMBindingExample Mac sample](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample)
to learn how to create a Mac binding using binding projects.

## Related Links

- [Binding Objective-C](~/cross-platform/macios/binding/index.md)
- [Binding iOS libraries](~/ios/platform/binding-objective-c/index.md)
