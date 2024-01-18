---
title: "Native Types for iOS and macOS"
description: "This document describes how Xamarin's Unified API maps .NET types to 32-bit and 64-bit native types, as necessary based on compilation target architecture."
ms.service: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: davidortinau
ms.author: daortin
ms.date: 01/25/2016
no-loc: [Objective-C]
---

# Native Types for iOS and macOS

Mac and iOS APIs use architecture-specific data types that are always
32 bits on 32-bit platforms and 64 bits on 64-bit platforms.

For example, Objective-C maps the `NSInteger` data type to `int32_t` on
32-bit systems and to `int64_t` on 64 bit systems.

To match this behavior, on our unified API, we are replacing the previous
uses of `int` (which in .NET is defined as always being `System.Int32`)
to a new data type: `System.nint`. You can think of the "n" as meaning
"native", so the native integer type of the platform.

With these new data types, the same source code is compiled for 32-bit and
64-bit architectures, depending on your compilation flags.

## New Data Types

The following table shows the changes in our data types to
match this new 32/64-bit world:

|Native Type|32-bit backing type|64-bit backing type|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

We chose those names to allow your C# code to look more or
less the same way that it would look today.

### Implicit and Explicit Conversions

The design of the new data types is intended to allow
a single C# source file to naturally use 32 or 64 bit storage
depending on the host platform and the compilation settings.

This required us to design a set of implicit and explicit
conversions to and from the platform-specific data types to
the .NET integral and floating point data types.

Implicit conversions operators are provided when there is
no possibility for data loss (32 bit values being stored on a
64 bit space).

Explicit conversions operators are provided when there is a
potential for data loss (64 bit value is being stored on a 32
or potentially 32 storage location).

`int`, `uint` and `float`
are all implicitly convertible
to `nint`, `nuint`
and `nfloat` as 32 bits will always fit in 32 or 64
bits.

`nint`, `nuint`
and `nfloat` are all implicitly convertible to `long`, `ulong` and `double`
as 32 or 64 bit values will always fit in 64 bit storage.

You must use explicit conversions
from `nint`, `nuint`
and `nfloat`
into `int`, `uint`
and `float` since the native types might hold 64
bits of storage.

You must use explicit conversions
from `long`, `ulong`
and `double`
into `nint`, `nuint`
and `nfloat` since the native types might only be
able to hold 32 bits of storage.

## CoreGraphics Types

The point, size and rectangle data types that are used with
CoreGraphics use 32 or 64 bits depending on the device they
are running on.  When we originally bound the iOS and Mac APIs
we used existing data structures that happened to match the
sizes of the host platform (The data types in `System.Drawing`).

When moving to **Unified**, you will need to replace instances of `System.Drawing` with their `CoreGraphics` counterparts as shown in the following table:

|Old Type in System.Drawing|New Data Type CoreGraphics|Description|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Holds floating point rectangle information.|
|`SizeF`|`CGSize`|Holds floating point size information (width, height)|
|`PointF`|`CGPoint`|Holds a floating point, point information (X, Y)|

The old data types used floats to store the elements of the
data structures, while the new one uses `System.nfloat`.

## Related Links

- [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs Unified API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
