---
title: ".NET Embedding Limitations"
description: "This document describes limitations of .NET Embedding, the tool that allows you to consume .NET code in other programming languages."
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
---

# .NET Embedding Limitations

This document explains the limitations of .NET Embedding and, whenever possible, provides workarounds for them.

## General

### Use more than one embedded library in a project

It is not possible to have two Mono runtimes co-existing inside the same application. This means you cannot use two different .NET Embedding-generated libraries inside the same application.

**Workaround:** You can use the generator to create a single library that includes several assemblies (from different projects).

### Subclassing

.NET Embedding eases the integration of the Mono runtime inside applications by exposing a set of ready-to-use APIs for the target language and platform.

However this is not a two-way integration, e.g. you cannot subclass a managed type and expect managed code to call back inside your native code, since your managed code is unaware of this co-existance.

Depending on your needs, it might be possible to workaround parts of this limitation, e.g.

* your managed code can p/invoke into your native code. This requires customizing your managed code to allow customization from native code;

* use products like Xamarin.iOS and expose a managed library that would allow Objective-C (in this case) to subclass some managed NSObject subclasses.

## Objective-C generated code

### Nullability

There is no metadata in .NET that tell us if a null reference is acceptable or not for an API. Most APIs will throw `ArgumentNullException` if they cannot cope with a `null` argument. This is problematic as Objective-C handling of exceptions is something better avoided.

Since we cannot generate accurate nullability annotations in the header files and wish to minimize managed exceptions we default to non-null arguments (`NS_ASSUME_NONNULL_BEGIN`) and add some specific, when precision is possible, nullability annotations.

### Bitcode (iOS)

Currently .NET Embedding does not support bitcode on iOS, which is enabled for some Xcode project templates. This will have to be disabled to successfully link generated frameworks.

![Bitcode Option](images/ios-bitcode-option.png)
