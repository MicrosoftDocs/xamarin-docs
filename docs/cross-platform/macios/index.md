---
title: "Apple Platform (iOS and Mac)"
description: "This document describes various topics related to Xamarin.iOS and Xamarin.Mac development: code sharing, the Unified API, binding Objective-C libraries, native references, native types, and more."
ms.service: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Apple Platform (iOS and Mac)

## Code Sharing

For elements of your code that have no user interface
elements the best way to share code between iOS and Mac is
still the use
of [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).

For code that has to do some user interface work and yet,
you want to share, you should
use [Shared Projects](~/cross-platform/app-fundamentals/shared-projects.md)
which allow you to place code to share in a
single project and have it compiled with both Mac and iOS when
referenced.

## [Unified API](unified/index.md)

The Unified API for iOS and Mac projects uses the same namespaces
for frameworks so that the same code file can be used across both
platforms, for seamless code-sharing. It also enables both 32 and 64 bit
builds. The Unified API has been the template default since early 2015,
and is recommended for all new projects - *only* Unified API projects
can be submitted to the App Store.

### Classic APIs

> [!NOTE]
> **Classic Profile Deprecation:** As new platforms are added in Xamarin.iOS we are starting to gradually deprecate features from the classic profile (monotouch.dll). For example, the non-NRC (new-ref-count) option was removed. NRC has always been enabled for all unified applications (i.e. non-NRC was never an option) and has no known issues. Future releases will remove the option of using Boehm as the garbage collector. This was also an option never available to unified applications. The complete removal of classic support is scheduled for fall 2016 with the release of Xamarin.iOS 10.0.

The original (non-Unified) Xamarin.iOS and Xamarin.Mac APIs made code-sharing
more difficult because native frameworks had either `MonoTouch.` or
`MonoMac.` namespace prefixes.  We provided some empty
namespaces that allows developers to share code by adding
`using` statements that reference both MonoMac and MonoTouch
namespaces on the same file, but this was a little ugly. The Classic API
should only continue to be used in legacy apps that are internally distributed
(upgrading to the Unified API is recommended).

### Updating from Classic to the Unified API

There are detailed instructions for updating any application from
the Classic to the Unified API.

## [Binding Objective-C Libraries](binding/index.md)

Xamarin lets you bring native libraries into your apps with bindings. This
section explains:

- how bindings work,
- how to manually build a binding project that lets you bring Objective-C code into Xamarin, and
- how to use our **Objective Sharpie** tool to help automate the process.

## [Native References](native-references.md)

## [Mac/iOS Native Types](nativetypes.md)

To support 32 and 64 bit code transparently from C# and F#,
we are introducing new data types.   Learn about them
here.

## [Building 32 and 64 bit apps](32-and-64/index.md)

What you need to know to support 32 and 64 bit
applications.

## [Working with Native Types in Cross-Platform Apps](native-types-cross-platform.md)

This article covers using the new iOS Unified API Native types
(`nint`, `nuint`, `nfloat`) in a cross-platform application where
code is shared with non-iOS devices such as Android or Windows Phone OSes.
It provides insight into when the Native types should be used and provides
several possible solutions to cases where the new type must be used with cross-platform code.

## [HttpClient Stack and SSL/TLS Implementation Selector](http-stack.md)

The new HttpClient Stack Selector controls which HttpClient implementation to use in your Xamarin.iOS, Xamarin.tvOS and Xamarin.Mac app. You can now switch to an implementation that uses iOS’s, tvOS's or OS X's native transports (`NSUrlSession` or `CFNetwork` depending on the OS).

SSL (Secure Socket Layer) and its successor, TLS (Transport Layer Security), provide support for HTTP and other network connections via `System.Net.Security.SslStream`. The new SSL/TLS implementation build option switches between Mono’s own TLS stack, and one powered by Apple’s TLS stack present in Mac and iOS.
