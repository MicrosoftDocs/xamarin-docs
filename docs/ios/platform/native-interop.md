---
title: "Referencing Native Libraries in Xamarin.iOS"
description: "This document discusses how to link native C libraries into a Xamarin.iOS application. It describes how to build universal native libraries and accessing C methods from C#."
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/28/2016
---

# Referencing Native Libraries in Xamarin.iOS

Xamarin.iOS supports linking with both native C libraries and Objective-C
libraries. This document discusses how to link your native C libraries with your
Xamarin.iOS project. For information on doing the same for Objective-C libraries,
see our [Binding Objective-C Types](~/ios/platform/binding-objective-c/index.md) document.

<a name="building_native"></a>

## Building Universal Native Libraries (i386, ARMv7, and ARM64)

It is often desirable to build your native libraries for each of the
supported platforms for iOS development (i386 for the Simulator and ARMv7/ARM64
for the devices themselves). If you've already got an Xcode project for your
library, this is really trivial to do.

To build the i386 version of your native library, run the following command
from a terminal:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

This will result in a native static library under `MyProject.xcodeproj/build/Release-iphonesimulator/`. Copy (or move)
the library archive file (libMyLibrary.a) to someplace safe for later use,
giving it a unique name (such as **libMyLibrary-i386.a**) so that it doesn't clash
with the arm64 and armv7 versions of the same library that you will build
next.

To build the ARM64 version of your native library, run the following
command:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

This time the built native library will be located in `MyProject.xcodeproj/build/Release-iphoneos/`. Once again, copy (or
move) this file to a safe location, renaming it to something like
**libMyLibrary-arm64.a** so that it won't clash.

Now build the ARMv7 version of the library:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Copy (or move) the resulting library file to the same location you moved the
other 2 versions of the library, renaming it to something like
**libMyLibrary-armv7.a**.

To make a universal binary, you can use the `lipo` tool like
so:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

This creates `libMyLibrary.a` which will be a universal (fat) library which
will be suitable to use for all iOS development targets.

### Missing Required Architecture i386

If you are getting a `does not implement methodSignatureForSelector` or `does not implement doesNotRecognizeSelector` message in your Runtime Output when trying to consume an Objective-C library in the iOS Simulator, your library probably was not compiled for the i386 architecture (see the [Building Universal Native Libraries](#building_native) section above).

To check the architectures supported by a given library, use the following command in the Terminal:

```bash
lipo -info /full/path/to/libraryname.a
```

Where `/full/path/to/` is the full path to the library being consumed and `libraryname.a` is the name of the library in question.

If you have the source to the library, you'll need to compile and bundle it for the i386 architecture as well, if you want to test your app on the iOS simulator.

### Linking Your Library

Any third-party library that you consume needs to be statically linked with
your application. 

If you wanted to statically link the library "libMyLibrary.a" that you got
from the Internet or build with Xcode, you would need to do a few things:

- Bring the Library into your project
- Configure Xamarin.iOS to link the library
- Access the methods from the library.

To **bring the library into your project**, Select the project
from the solution explorer and press **Command+Option+a**. Navigate to the
libMyLibrary.a and add it to the project. When prompted, tell Visual Studio for Mac or Visual Studio to
copy it into the project. After adding it, find the libFoo.a in the project,
right click on it, and set the **Build Action** to **none**.

To **Configure Xamarin.iOS To Link the Library**, on the project
options for your final executable (not the library itself, but the final
program) you need to add in **iOS Build**'s Extra argument (these are part of
your project options) the "-gcc_flags" option followed by a quoted string that
contains all the extra libraries that are required for your program, for
example:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

The above example will link **libMyLibrary.a**

You can use `-gcc_flags` to specify any set of command line arguments to
pass to the GCC compiler used to do the final link of your executable. For
example, this command line, also references the CFNetwork framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

If your native library contains C++ code you must also pass the -cxx flag in
your "Extra Arguments" so that Xamarin.iOS knows to use the correct compiler. For
C++ the previous options would look like:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#"></a>

## Accessing C Methods from C&#35;

There are two kinds of native libraries available on iOS:

- Shared libraries that are part of the operating system.

- Static libraries that you ship with your application.

To access methods defined in either one of those, you use [Mono's P/Invoke functionality](https://www.mono-project.com/docs/advanced/pinvoke/) which is the same technology that you
would use in .NET, which is roughly:

- Determine which C function you want to invoke
- Determine its signature
- Determine which library it lives in
- Write the appropriate P/Invoke declaration

When you use P/Invoke you need to specify the path of the library that you
are linking with. When using iOS shared libraries, you can either hardcode the
path, or you can use the convenience constants that we have defined in our `Constants`, these constants should cover the iOS shared libraries.

For example, if you wanted to invoke the UIRectFrameUsingBlendMode method
from Apple's UIKit library which has this signature in C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Your P/Invoke declaration would look like this:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

The Constants.UIKitLibrary is merely a constant defined as
"/System/Library/Frameworks/UIKit.framework/UIKit", the EntryPoint lets us
specify optionally the external name (UIRectFramUsingBlendMode) while exposing a
different name in C#, the shorter RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs"></a>

### Accessing C Dylibs

If you have the need to consume a C Dylib in your Xamarin.iOS application, there is a bit of extra setup that is required before calling the `DllImport` attribute.

For example, if we have an `Animal.dylib` with an `Animal_Version` method that we will be calling in our application, we need to inform Xamarin.iOS of the location of the library before trying to consume it.

To do this, edit the `Main.CS` file and make it look like the following:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Where `/full/path/to/` is the full path to the Dylib being consumed. With this code in place, we can then link to the `Animal_Version` method as follows:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries"></a>

### Static Libraries

Since you can only use static libraries on iOS, there is no external shared
library to link with, so the path parameter in the DllImport attribute needs to
use the special name `__Internal` (note the double underscore characters at the start of the name) as opposed to the path name.

This forces DllImport to look up the symbol of the method that you are
referencing in the current program, instead of trying to load it from a shared
library.
