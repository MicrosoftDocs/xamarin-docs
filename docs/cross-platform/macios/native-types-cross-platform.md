---
title: "Working with Native Types in Cross-Platform Apps"
description: "This article covers using the new iOS Unified API Native types (nint, nuint, nfloat) in a cross-platform application where code is shared with non-iOS devices such as Android or Windows Phone OSes."
ms.service: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: davidortinau
ms.author: daortin
ms.date: 04/07/2016
no-loc: [Objective-C]
---

# Working with Native Types in Cross-Platform Apps

_This article covers using the new iOS Unified API Native types (nint, nuint, nfloat) in a cross-platform application where code is shared with non-iOS devices such as Android or Windows Phone OSes._

The 64-types native types work with the iOS and Mac APIs. If you are writing shared code that runs on Android or Windows as well, you'll need to manage the conversion of Unified types into regular .NET types that you can share.

This document discusses different ways to interoperate with the Unified API from your shared/common code.

## When to Use the Native Types

Xamarin.iOS and Xamarin.Mac Unified APIs still include the `int`, `uint` and `float` data types, as well as the `RectangleF`, `SizeF` and `PointF` types. These existing data types should continue to be used in any shared, cross-platform code. The new Native data types should only be used when making a call to a Mac or iOS API where support for architecture-aware types are required.

Depending on the nature of the code being shared, there might be times where cross-platform code might need to deal with the `nint`, `nuint` and `nfloat` data types. For example: a library that handles transformations on rectangular data that was previously using `System.Drawing.RectangleF` to share functionality between Xamarin.iOS and Xamarin.Android versions of an app, would need to be updated to handle Native Types on iOS.

How these changes are handled depends on the size and complexity of the application and the form of code sharing that has been used, as we will see in the following sections.

## Code Sharing Considerations

As stated in the [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) document, there are two main ways to sharing code between cross-platform projects: Shared Projects and Portable Class Libraries. Which of the two types has been used, will limit the options we have when handling the Native data types in cross-platform code.

### Portable Class Library Projects

A Portable Class Library (PCL) allows you to target the platforms you wish to support, and use interfaces to provide platform-specific functionality.

Since the PCL Project type is compiled down to a `.DLL` and it has no sense of the Unified API, you'll be forced to keep using the existing data types (`int`, `uint`, `float`) in the PCL source code and type cast the calls to the PCL's classes and methods in the front-end applications. For example:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### Shared Projects

The Shared Asset Project type allows you to organize your source code in a separate project that then gets included and compiled into the individual platform-specific front end apps, and use `#if` compiler directives as required to manage platform-specific requirements.

The size and complexity of the front end mobile applications that are consuming shared code, along with the size and the complexity of the code being shared, needs to be taken into account when choosing the method of support for Native data types in a cross-platform Shared Asset Project.

Based on these factors, the following types of solutions might be implemented using the `if __UNIFIED__ ... #endif` compiler directives to handle the Unified API specific changes to the code.

#### Using Duplicate Methods

Take the example of a library that is doing transformations on rectangular data given above. If the library only contains one or two very simple methods, you might choose to create duplicate versions of those methods for Xamarin.iOS and Xamarin.Android. For example:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

In the above code, since the `CalculateArea` routine is very simple, we have used conditional compilation and created a separate, Unified API version of the method. On the other hand, if the library contained many routines or several complex routines, this solution would not be feasible, as it would present an issue keeping all of the methods in sync for modifications or bug fixes.

#### Using Method Overloads

In that case, the solution might be to create an overload version of the methods using 32-bit data types so that they now take `CGRect` as a parameter and/or a return value, convert that value to a `RectangleF` (knowing that converting from `nfloat` to `float` is a lossy conversion), and call the original version of the routine to do the actual work. For example:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Call original routine to calculate area
                return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Again, this is a good solution as long as the loss of precision doesn't affect the results for your application's specific needs.

#### Using Alias Directives

For areas where the loss of precision is an issue, another possible solution is to use `using` directives to create an alias for Native and CoreGraphics data types by including the following code to the top of the shared source code files and converting any needed `int`, `uint` or `float` values to `nint`, `nuint` or `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

So that our example code then becomes:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Note that here we have changed the `CalculateArea` method to return an `nfloat` instead of the standard `float`. This was done so that we would not get a compile error trying to _implicitly_ convert the `nfloat` result of our calculation (since both values being multiplied are of type `nfloat`) into a `float` return value.

If the code is compiled and run on a non Unified API device, the `using nfloat = global::System.Single;` maps the `nfloat` to a `Single` which will implicitly convert to a `float` allowing the consuming front-end application to call the `CalculateArea` method without modification.

#### Using Type Conversions in the Front End App

In the event that your front end applications only make a handful of calls to your shared code library, another solution could be to leave the library unchanged and do type casting in the Xamarin.iOS or Xamarin.Mac application when calling the existing routine. For example:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

If the consuming application makes hundreds of calls to the shared code library, this again, might not be a good solution.

Based on our application's architecture, we might end up using one or more of the above solutions to support Native Data Types (where required) in our cross-platform code.

## Xamarin.Forms Applications

The following is required to use Xamarin.Forms for cross-platform UIs that will also be shared with a Unified API application:

- The entire solution must be using version 1.3.1 (or greater) of the Xamarin.Forms NuGet Package.
- For any Xamarin.iOS custom renders, use the same types of solutions presented above based on how the UI code has been shared (Shared Project or PCL).

As in a standard cross-platform application, the existing 32-bit data types should be used in any shared, cross-platform code for most situations. The new Native Data Types should only be used when making a call to a Mac or iOS API where support for architecture-aware types is required.

For more details, see our [Updating Existing Xamarin.Forms Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) documentation.

## Summary

In this article we have seen when to use the Native Data Types in a Unified API application and their implications cross-platform. We have presented several solutions that can be used in situations where the new Native Data Types must be used in cross-platform libraries. Also, we've seen a quick guide to supporting Unified APIs in Xamarin.Forms cross-platform applications.

## Related Links

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Native Types](~/cross-platform/macios/nativetypes.md)
- [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md)
- [Code Sharing Sample](/samples/xamarin/mobile-samples/sharingcode/)