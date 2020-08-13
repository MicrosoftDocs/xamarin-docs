---
title: "Binding iOS Libraries"
description: "This document describes how to create C# bindings to Objective-C code, making it possible to consume native libraries and CocoaPods in a Xamarin.iOS application."
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
---

# Binding iOS Libraries

> [!IMPORTANT]
> We're currently investigating custom binding usage on the Xamarin platform. Please take [**this survey**](https://www.surveymonkey.com/r/KKBHNLT) to inform future development efforts.

Follow these links to learn about binding Objective-C libraries and CocoaPods
for Xamarin.iOS and Xamarin.Mac:

- [**Overview**](~/cross-platform/macios/binding/overview.md) -
  Describes how binding works.
- [**Binding Objective-C Libraries**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Instructions on how to bind Objective-C libraries for use in Xamarin projects.
- [**Type Definition Reference Guide**](~/cross-platform/macios/binding/binding-types-reference.md) -
  Describes all of the attributes available to binding authors to drive the binding
  generation process.

## [Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objective Sharpie is a command line tool to help bootstrap the first pass of a binding.
It works by parsing the header files of a native library to map the public API
into the [binding definition](~/cross-platform/macios/binding/objective-c-libraries.md)
(a process that is otherwise done manually). Objective Sharpie does not create
a binding by itself, but it can help get you started!

Objective Sharpie 3.0 introduced the ability to bind Cocoapods directly!

## [Walkthrough - Binding an iOS Objective-C Library](walkthrough.md)

This page provides a step-by-step walkthrough of creating an iOS binding project
using the open source [**InfColorPicker**](https://github.com/InfinitApps/InfColorPicker)
Objective-C project as an example. The **InfColorPicker** library provides a reusable
view controller that allow the user to select a color based on its HSB
representation, making color selection more user-friendly.
Objective Sharpie will be used to assist in the binding process.

## Video

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS Bindings in C/C++ video**

## Related Links

- [Binding Objective-C](~/cross-platform/macios/binding/index.md)
- [Mac Binding](~/mac/platform/binding.md)
