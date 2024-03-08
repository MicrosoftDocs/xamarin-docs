---
title: "tvOS in Xamarin – Internals"
description: "Documents describing the internal workings of tvOS on Xamarin, which is based on Xamarin.iOS. Link content discusses assemblies, target frameworks, and related iOS concepts."
ms.service: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
no-loc: [Objective-C]
---

# tvOS in Xamarin – Internals 

## [Assemblies](~/ios/tvos/internals/assemblies.md)

List of the assemblies supported by Xamarin for your Xamarin.tvOS applications.

## [Target Frameworks](~/ios/tvos/internals/frameworks.md)

This article covers the types of Target Frameworks (Base Class Libraries) that are available in Xamarin.tvOS and the implications of selecting a specific target for your Xamarin.tvOS application.

## Related iOS Articles

The following articles are specific to iOS but relevant to tvOS (since tvOS 9 is a subset of iOS 9).

### [Unified API](~/cross-platform/macios/unified/index.md)

Introduces the new Unified APIs which allow for simpler code sharing between Apple TV and iOS codebases as well as introducing support for 64-bit APIs and 64-bit compilation.  

### [API Design](~/ios/internals/api-design/index.md)

Explains the design principles behind the API Binding.

### [Limitations](~/ios/internals/limitations.md)

This section illustrates pitfalls and limitations to be aware of with regards Xamarin.iOS, many of which are applicable to Xamarin.tvOS.

### [Linker](~/ios/deploy-test/linker.md)

Explains how the linker works to ensure the smallest possible application package, as well as how to modify it's settings and usage.

### [Localization and Internationalization](~/ios/app-fundamentals/localization/index.md)

This guide covers the addition of encodings to a Xamarin.iOS application to support internationalization.

### [mtouch](~/ios/deploy-test/mtouch.md)

Notes and information on mtouch.exe, the command line tool that builds your project into an application usable by iOS.

### [Linking Native Libraries](~/ios/platform/native-interop.md)

Xamarin.iOS supports linking with both native C libraries and Objective-C libraries. This document discusses how to link your native C libraries with your Xamarin.iOS project. For information on doing the same for Objective-C libraries, see the&nbsp; [Binding Objective-C Types](~/ios/platform/binding-objective-c/index.md)&nbsp;document.

## [Objective-C Selectors](~/ios/internals/objective-c-selectors.md)

Notes and usage for calling Objective-C Selectors (methods) directly.

### [System.Data](~/ios/data-cloud/system.data.md)

Information and instructions on using System.Data to access the built-in SQLite Database system.

### [Threading](~/ios/app-fundamentals/threading.md)

Notes on using threading within Xamarin.iOS applications.

### [XIB Code Generation](~/ios/internals/xib-code-generation.md)

How Visual Studio for Mac integrates with Xcode's Interface Builder to allow you to use Interface Builder to design UI.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)