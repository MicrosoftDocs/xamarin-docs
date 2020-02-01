---
title: "Objective Sharpie Release History"
description: "This document describes the release history of Objective Sharpie, the tool used to automate the creation of C# bindings to Objective-C code."
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
---

# Objective Sharpie Release History

## 3.4 (October 11, 2017)

[Download v3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Support for Xcode 9: iOS 11, macOS 10.13, tvOS 11, and watchOS 4
* Issues with SIMD and tgmath should now be fixed
* Telemetry has been removed completely

## 3.3 (August 3, 2016)

[Download v3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Support for Xcode 8 Beta 4, iOS 10, macOS 10.12, tvOS 10 and watchOS 3.
* Updated to latest Clang master build (2016-08-02)
* [Persist telemetry submission options](https://twitter.com/Symbiatch/status/760373403878559744) from `sharpie pod bind` to `sharpie bind`.

## 3.2 (June 14, 2016)

[Download v3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Support for Xcode 8 Beta 1, iOS 10, macOS 10.12, tvOS 10 and watchOS 3.

## 3.1 (May 31, 2016)

[Download v3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Support for CocoaPods 1.0
* Improved CocoaPods binding reliability by first building a native `.framework` and then binding that
* Copy CocoaPods `.framework` and binding definition into a `Binding` directory to make integration with Xamarin.iOS and Xamarin.Mac binding projects easier

## 3.0 (October 5, 2015)

[Download v3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Support for new Objective-C language features including lightweight generics and nullability, as introduced in Xcode 7
* Support for the latest iOS and Mac SDKs.
* Xcode project building and parsing support! You can now pass a full Xcode project to Objective Sharpie and it will do its best to figure out the right thing to do (e.g. `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) support! You can now configure, build, and bind CocoaPods directly from Objective Sharpie (e.g. `sharpie pod init ios AFNetworking && sharpie pod bind`).
* When binding frameworks, Clang modules will be enabled if the framework supports them, resulting in the most correct parsing of a framework, since the structure of the framework is defined by its `module.modulemap`.
* For Xcode projects that build a framework product, parse that product instead of intermediate product targets as non-framework targets in an Xcode project may still have ambiguities which cannot be automatically resolved.

## 2.1.6 (March 17, 2015)

* Fixed binary operator expression binding: the left-hand side of the expression was incorrectly swapped with the right-hand (e.g. `1 << 0` was incorrectly bound as `0 << 1`). Thanks to Adam Kemp for noticing this!
* Fixed an issue with `NSInteger` and `NSUInteger` being bound as `int` and `uint` instead of `nint` and `nuint` on i386; `-DNS_BUILD_32_LIKE_64` is now passed to Clang to make parsing `objc/NSObjCRuntime.h` work as expected on i386.
* The default architecture for Mac OS X SDKs (e.g. `-sdk macosx10.10`) is now x86_64 instead of i386, so `-arch` can be omitted unless overriding the default is desired.

## 2.1.0 (March 15, 2015)

[Download v2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Ensure `using ObjCRuntime;` is produced when `ArgumentSemantic` is used.
* [bxc#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Ensure `using System.Runtime.InteropServices;` is produced when `DllImport` is used.
* [bxc#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): Default `DllImport` to loading symbols from `__Internal`.
* [bxc#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Skip forward-declared Objective-C container declarations.
* [bxc#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Bind protocol types with a single qualification as concrete interfaces (`id<Foo>` as `Foo` instead of `Foundation.NSObject<Foo>`).
* [bxc#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): Bind `UInt32`, `UInt64`, and `Int64` literals as `Int32` to drop the `u` and/or `uL` suffixes when the values can safely fit into `Int32`.
* [bxc#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Fix enum name mapping when original native name starts with a `k` prefix.
* `sizeof` C expressions whose argument type does not map to a C# primitive type will be evaluated in Clang and bound as an integer literal to avoid generating invalid C#.
* Fix Objective-C syntax for properties whose type is a block (Objective-C code appears in comments above bound declarations).
* Bind decayed types as their original type (`int[]` decays to `int*` during semantic analysis in Clang, but bind it as the original as-written `int[]` instead).

Thanks very much to Dave Dunkin for reporting many of the bugs fixed in this point release!

## 2.0.0: March 9, 2015

[Download v2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Objective Sharpie 2.0 is a major release that features an improved Clang-based driver and parser and a new NRefactory-based binding engine. These improved components provide for _much_ better bindings, particularly around type binding. Many other improvements have been made that are internal to Objective Sharpie which will yield many user-visible features in future releases.

Objective Sharpie 2.0 is based on Clang 3.6.1.

### Type binding improvements

* Objective-C blocks are now supported. This includes anonymous/inline blocks and blocks named via `typedef`. Anonymous blocks will be bound as `System.Action` or `System.Func` delegates, while named blocks will be bound as strongly named `delegate` types.

* There is an improved naming heuristic for anonymous enums that are immediately preceded by a `typedef` resolving to a builtin integral type such as `long` or `int`.

* C pointers are now bound as C# `unsafe` pointers instead of `System.IntPtr`. This results in more clarity in the binding for when you may wish to turn pointer parameters into `out` or `ref` parameters. It is not possible to always infer whether a parameter should be `out` or `ref`, so the pointer is retained in the binding to allow for easier auditing.

* An exception to the above pointer binding is when a 2-rank pointer to an Objective-C object is encountered as a parameter. In these cases, convention is predominant and the parameter will be bound as `out` (e.g. `NSError **error` → `out NSError error`).

### Verify attribute

You will often find that bindings produced by Objective Sharpie will now be annotated with the `[Verify]` attribute. These attributes indicate that you should _verify_ that Objective Sharpie did the correct thing by comparing the binding with the original C/Objective-C declaration (which will be provided in a comment above the bound declaration).

Verification is _recommended_ for all bound declarations, but is most likely _required_ for declarations annotated with the `[Verify]` attribute. This is because in many situations, there is not enough metadata in the original native source code to infer how to best produce a binding. You may need to reference documentation or code comments inside the header files to make the best binding decision.

See the [Verify Attributes](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) documentation for more details.

### Other notable improvements

* `using` statements are now generated based on types bound. For instance, if an `NSURL` was bound, a `using Foundation;` statement will be generated as well.

* `struct` and `union` declarations will now be bound, using the `[FieldOffset]` trick for unions.

* Enum values with constant expression initializers will now be properly bound; the full expression is translated to C#.

* Variadic methods and blocks are now bound.

* Frameworks are now supported via the `-framework` option. See the documentation on [Binding Native Frameworks](~/cross-platform/macios/binding/objective-sharpie/index.md) for more details.

* Objective-C source code will be auto-detected now, which should eliminate the need to pass `-ObjC` or `-xobjective-c` to Clang manually.

* Clang module usage (`@import`) is now auto-detected, which should eliminate the need to pass `-fmodules` to Clang manually for libraries which use the new module support in Clang.

* The Xamarin Unified API is now the default binding target; use the `-classic` option to target the 32-bit only Classic API.

### Notable bug fixes

* Fix `instancetype` binding when used in an Objective-C category
* Fully name Objective-C categories
* Prefix Objective-C protocols with `I` (e.g. `INSCopying` instead of `NSCopying`)

## 1.1.35: December 21, 2014

[Download v1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Minor bug fixes.

## 1.1.1: December 15, 2014

[Download v1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 was the first major release after 1.5 years of internal use and development at Xamarin following the initial preview of Objective Sharpie in April 2013. This release is the first to be generally considered stable and usable for a wide variety of native libraries, featuring a new Clang backend.
