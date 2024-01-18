---
title: "Migrating a Binding to the Unified API"
description: "This article covers the steps required to update an existing Xamarin Binding Project to support the Unified APIs for Xamarin.IOS and Xamarin.Mac applications."
ms.service: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Migrating a Binding to the Unified API

_This article covers the steps required to update an existing Xamarin Binding Project to support the Unified APIs for Xamarin.IOS and Xamarin.Mac applications._

## Overview

Starting February 1st, 2015 Apple requires that all new submissions to the iTunes and the Mac App Store must be 64 bit applications. As a result, any new Xamarin.iOS or Xamarin.Mac application will need to be using the new Unified API instead of the existing Classic MonoTouch and MonoMac APIs to support 64 bit.

Additionally, any Xamarin Binding Project must also support the new Unified APIs to be included in a 64 bit Xamarin.iOS or Xamarin.Mac project. This article will cover the steps required to update an existing binding project to use the Unified API.

## Requirements

The following is required to complete the steps presented in this article:

- **Visual Studio for Mac** - The latest version of Visual Studio for Mac installed and configured on the development computer.
- **Apple Mac** - An Apple mac is required to build Binding Projects for iOS and Mac.

Binding projects are not supported in Visual studio on a Windows machine.

## Modify the Using Statements

The Unified APIs makes it easier than ever to share code between Mac and iOS as well as allowing you to support 32 and 64 bit applications with the same binary. By dropping the _MonoMac_ and _MonoTouch_ prefixes from the namespaces, simpler sharing is achieved across Xamarin.Mac and Xamarin.iOS application projects.

As a result we will need to modify any of our binding contracts (and other `.cs` files in our binding project) to remove the _MonoMac_ and _MonoTouch_ prefixes from our `using` statements.

For example, given the following using statements in a binding contract:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

We would strip off the `MonoTouch` prefix resulting in the following:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Again, we will need to do this for any `.cs` file in our binding project. With this change in place, the next step is to update our binding project to use the new Native Data Types.

For more information on the Unified API, please see the [Unified API](~/cross-platform/macios/unified/index.md) documentation. For more background on supporting 32 and 64 bit applications and information about frameworks see the [32 and 64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md) documentation.

## Update to Native Data Types

Objective-C maps the `NSInteger` data type to `int32_t` on 32 bit systems and to `int64_t` on 64 bit systems. To match this behavior, the new Unified API replaces the previous uses of `int` (which in .NET is defined as always being `System.Int32`) to a new data type: `System.nint`.

Along with the new `nint` data type, the Unified API introduces the `nuint` and `nfloat` types, for mapping to the `NSUInteger` and `CGFloat` types as well.

Given the above, we need to review our API and ensure that any instance of `NSInteger`, `NSUInteger` and `CGFloat` that we previously mapped to `int`, `uint` and `float` get updated to the new `nint`, `nuint` and `nfloat` types.

For example, given an Objective-C method definition of:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

If the previous binding contract had the following definition:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

We would update the new binding to be:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```

If we are mapping to a newer version 3rd party library than what we had initially linked to, we need to review the `.h` header files for the library and see if any exiting, explicit calls to `int`, `int32_t`, `unsigned int`, `uint32_t` or `float` have been upgraded to be an `NSInteger`, `NSUInteger` or a `CGFloat`. If so, the same modifications to the `nint`, `nuint` and `nfloat` types will need to be made to their mappings as well.

To learn more about these data type changes, see the [Native Types](~/cross-platform/macios/nativetypes.md) document.

## Update the CoreGraphics Types

The point, size and rectangle data types that are used with `CoreGraphics` use 32 or 64 bits depending on the device they are running on. When Xamarin originally bound the iOS and Mac APIs we used existing data structures that happened to match the data types in `System.Drawing` (`RectangleF` for example).

Because of the requirements to support 64 bits and the new native data types, the following adjustments will need to be made to existing code when calling `CoreGraphic` methods:

- **CGRect** - Use `CGRect` instead of `RectangleF` when defining floating point rectangular regions.
- **CGSize** - Use `CGSize` instead of `SizeF` when defining floating point sizes (width and height).
- **CGPoint** - Use `CGPoint` instead of `PointF` when defining a floating point location (X and Y coordinates).

Given the above, we will need to review our API and ensure that any instance of `CGRect`, `CGSize` or `CGPoint` that was previously bound to `RectangleF`, `SizeF` or `PointF` be changed to the native type `CGRect`, `CGSize` or `CGPoint` directly.

For example, given an Objective-C initializer of:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

If our previous binding included the following code:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

We would update that code to:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

With all of the code changes now in place, we need to modify our binding project or make file to bind against the Unified APIs.

## Modify the Binding Project

As the final step to updating our binding project to use the Unified APIs, we need to either modify the `MakeFile` that we use to build the project or the Xamarin Project Type (if we are binding from within Visual Studio for Mac) and instruct _btouch_ to bind against the Unified APIs instead of the Classic ones.

### Updating a MakeFile

If we are using a makefile to build our binding project into a Xamarin .DLL, we will need to include the `--new-style` command line option and call `btouch-native` instead of `btouch`.

So given the following `MakeFile`:

<!--markdownlint-disable MD010 -->
```makefile
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch

all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
	$(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
	-mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
	$(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
	-mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
	$(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
	-mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
	lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
	$(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
	-rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

We need to switch from calling `btouch` to `btouch-native`, so we would adjust our macro definition as follows:

```makefile
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

We would update the call to `btouch` and add the `--new-style` option as follows:

<!--markdownlint-disable MD010 -->
```makefile
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
	$(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```
<!--markdownlint-enable MD010 -->

We can now execute our `MakeFile` as normal to build the new 64 bit version of our API.

### Updating a Binding Project Type

If we are using a Visual Studio for Mac Binding Project Template to build our API, we'll need to update to the new Unified API version of the Binding Project Template. The easiest way to do this is to start a new Unified API Binding Project and copy over all of the existing code and settings.

Do the following:

1. Start Visual Studio for Mac.
2. Select **File** > **New** > **Solution...**
3. In the New Solution Dialog Box, select **iOS** > **Unified API** > **iOS Binding Project**: 

    [![In the New Solution Dialog Box, select iOS / Unified API / iOS Binding Project](update-binding-images/image01new.png)](update-binding-images/image01new.png#lightbox)
4. On 'Configure your new project' dialog enter a **Name** for the new binding project and click the **OK** button.
5. Include the 64 bit version of Objective-C library that you are going to be creating bindings for.
6. Copy over the source code from your existing 32 bit Classic API binding project (such as the `ApiDefinition.cs` and `StructsAndEnums.cs` files).
7. Make the above noted changes to the source code files.

With all of these changes in place, you can build the new 64 bit version of the API as you would the 32 bit version.

## Summary

In this article we have shown the changes that need to be made to an existing Xamarin Binding Project to support the new Unified APIs and 64 bit devices and the steps required to build the new 64 bit compatible version of an API.

## Related Links

- [Mac and iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md)
- [Upgrading Existing iOS Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](/samples/xamarin/ios-samples/bindingsample/)