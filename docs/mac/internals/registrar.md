---
title: "Xamarin.Mac registrar"
description: "This document describes the purpose of the Xamarin.Mac registrar and its dynamic, static, and partial static (hybrid) usage configurations."
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
---

# Xamarin.Mac registrar

_This document describes the purpose of the Xamarin.Mac registrar and its different usage configurations._

## Overview

Xamarin.Mac bridges the gap between the managed (.NET) world and Cocoa's runtime, allowing managed classes to call unmanaged Objective-C classes and be called back when events occur. The work required to preform this “magic” is handled by the registrar and is, in general, hidden from view.

There are performance implications of this registration, specifically on application start up time, and understanding a bit of what's going on "under the hood" can sometimes be helpful.

## Configurations

Fundamentally the registrar’s job at startup can be separated into two catagories:

- Scan every managed class for those deriving from NSObject and collect a list of items to be exposed to the Objective-C runtime.
- Register this information with the Objective-C runtime.

Over time, three different registrar configurations have been created to cover different use cases. Each has different build and run time consequences:

- **Dynamic registrar** – During startup, use .NET reflection to scan every loaded type, determine the list of relevant items, and inform the native runtime. This option adds zero time to the build but is very expensive to compute during launch (up to multiple seconds).
- **Static registrar** – During build, compute the set of items to be registered and generate Objective-C code to handle registration. This code is invoked during startup to quickly register all items. Adds a significant pause to build but can cut a significant amount of time from application start.
- **"Partial" static** – A newer "hybrid" approach which brings most of the advantages of both. Since the exports from **Xamarin.Mac.dll** are constant, save a precomputed library to handle their registration and link that in. Use reflection to handle user libraries, but as user libraries export much fewer types that the platform bindings this is often rather quick. A neglectable build time impact and reduces a vast majority of the “cost” of dynamic.

Today partial static is the default for Debug configuration and Static is the default for Release configurations.

There are some scenarios:

- Plugins loaded after launch with classes deriving from NSObject
- Dynamically created class instances deriving from NSObject

where the registrar is unable to know that it needs to register some type at start. The `ObjCRuntime.Runtime.RegisterAssembly` method is provided to inform the registrar that it has additional types to consider.
