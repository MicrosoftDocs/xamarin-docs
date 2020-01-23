---
title: "AndroidX Migration in Xamarin.Forms"
description: "This article explains why AndroidX exists and how to migrate to AndroidX in your Xamarin.Forms app."
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/22/2020
---

# AndroidX migration in Xamarin.Forms

AndroidX replaces the Android Support Library. This article explains why AndroidX exists, how it impacts Xamarin.Forms, and how to migrate your application to use the AndroidX libraries.

## History of AndroidX

The Android Support Library was created to provide newer features on older versions of Android. It is a compatibility layer that allows developers to use functionality that may not exist on all versions of the Android operating system and have graceful fallbacks for older versions. The Support Library also includes convenience and helper classes, debugging and utility tools, and sophisticated classes that depend on other Support Library classes to function.

While the Support Library was originally a single binary, it has grown and evolved into a suite of libraries which are almost essential for modern app development. These are some commonly used features from the Support Library:

- The `Fragment` support class.
- The `RecyclerView`, used for managing long lists.
- Multidex support for apps with over 65,536 methods.
- The `ActivityCompat` class.

AndroidX is a replacement for the Support Library, which is no longer maintained. AndroidX is a redesigned library that uses semantic versioning, clearer package names, and better support for common application architecture patterns. AndroidX version 1.0.0 is binary equivalent to Support Library version 28.0.0. All new library development will occur in the AndroidX library. For a complete list of class mappings from Support Library to AndroidX, see [Support Library class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) on developer.android.com.

With AndroidX, Google created a migration process called the Jetifier. The Jetifier inspects the jar bytecode during the build process and remaps Support Library references, both in app code and in dependencies, to their AndroidX equivalent.

In a Xamarin.Forms app, the jar dependencies must be migrated to AndroidX similar to an Android Java app. However, the Xamarin bindings must also be migrated to point to the correct, underlying jar files. Xamarin.Forms added support for automatic AndroidX migration in version 4.5.

## Automatic Migration in Xamarin.Forms

To automatically migrate to AndroidX, your project must:

- Target Android API version 29 or greater.
- Use Xamarin.Froms version 4.5 or greater.
- Be using Support Libraries directly or via a depedency.


## Manual Migration in Xamarin.Forms

## Related links

- [Android Support Library overview](https://developer.android.com/topic/libraries/support-library/index) on developer.android.com
- [AndroidX overview](https://developer.android.com/jetpack/androidx) on developer.android.com