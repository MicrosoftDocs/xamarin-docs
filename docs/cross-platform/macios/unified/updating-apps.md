---
title: "Updating Existing Apps to the Unified API"
description: "This document links to various guides that describe how to update Xamarin applications to the Unified API. It discusses Xamarin.iOS apps, Xamarin.Mac apps. Xamarin.Forms apps, native types in cross-platform apps, and binding projects."
ms.service: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Updating Existing Apps to the Unified API

> [!IMPORTANT]
> The Xamarin Classic API, which preceded the Unified API, has been
> deprecated.
>
> - The last version of Xamarin.iOS to support the Classic API
>   (monotouch.dll) was Xamarin.iOS 9.10.
> - Xamarin.Mac still supports the Classic API, but it is no longer
>   updated. Since it is deprecated, developers should move their
>   applications to the Unified API.

## How to Update Your Apps

There are three steps to update your apps:

1. Fix any compiler warnings in your existing code,
    particularly those relating to deprecated APIs.

2. Use the Migration Tool built in to Visual Studio for Mac
    to update your project files and namespaces.

3. Fix remaining compiler errors relating to the new
    [64-types](~/cross-platform/macios/nativetypes.md)
    and [other APIs](~/cross-platform/macios/unified/overview.md#deprecated-typos)
    that have changed. Check out [these tips](~/cross-platform/macios/unified/updating-tips.md)
    for additional information on manual updates that
    might be required.

There are specific guides available for each product to help you update
your apps to the Unified API and 64-bit support:

### [Xamarin.iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md)

Existing Xamarin.iOS apps can be updated to the Unified API using
the automated migration tool built in to Visual Studio for Mac. Some additional
fixes may then be required, as explained in [these instructions](~/cross-platform/macios/unified/updating-ios-apps.md)
and [tips](~/cross-platform/macios/unified/updating-tips.md).

### [Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Existing Xamarin.Mac apps can be updated to the Unified API using
the automated migration tool built in to Visual Studio for Mac. Some additional
fixes may then be required, as explained in [these instructions](~/cross-platform/macios/unified/updating-mac-apps.md)
and [tips](~/cross-platform/macios/unified/updating-tips.md).

### [Xamarin.Forms apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Follow these instructions to update an existing Xamarin.Forms
solution with an iOS project to use the Unified API. Unified API
support is only available in Xamarin.Forms 1.3 and later, so
[the instructions](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) also explain how
to update your Xamarin.Forms app to version 1.3. These [tips](~/cross-platform/macios/unified/updating-tips.md)
may help updating any native iOS code in custom renderers or
dependency services.

## [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/nativetypes.md)

This article covers using the new iOS Unified API Native types (nint, nuint, nfloat) in a cross-platform application where code is shared with non-iOS devices such as Android or Windows Phone OSes. It provides insight into when the Native types should be used and provides several possible solutions to cases where the new type must be used with cross-platform code.

## Update Bindings to the Unified API

Customers that have created bindings to Objective-C libraries
will need to update the binding project to reflect changes
in the underlying API (where some types will now be 64-bit).
Follow these instructions to [update an existing Binding Project to support the Unified API](~/cross-platform/macios/unified/update-binding.md).

## Related Links

- [Updating iOS Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Updating Mac Apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Updating Xamarin.Forms Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Updating Bindings](~/cross-platform/macios/unified/update-binding.md)
- [Updating Tips](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs Unified API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
