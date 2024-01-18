---
title: "Floating Point Operations in Xamarin.iOS"
description: "This document describes how Xamarin.iOS handles 32-bit and 64-bit precision floating point operations and discusses associated impacts to performance."
ms.service: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
no-loc: [Objective-C]
---

# Floating Point Operations in Xamarin.iOS

Xamarin.iOS will by default perform 32-bit and 64-bit floating point
operations using 64-bit precision on ARM.  

While this higher precision is closer to what developers expect from
floating point operations in C# on the desktop, on mobile, the
performance impact can be significant.

It is possible to compile your 32-bit floating point code to use 32-bit
floating point operations.  To do this, you can either uncheck the "Perform
all 32-bit float operations as 64-float." option in the iOS Build property
page in Visual Studio, or set the `MtouchFloat32` property in the project file
to `true` (create the property if it doesn't already exist):

```xml
<MtouchFloat32>true</MtouchFloat32>
```

This will inform the static compilers (either Mono's built-in static
compiler, or the LLVM-powered one) to perform floating point
operations using 32-bit floats.
