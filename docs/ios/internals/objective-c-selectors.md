---
title: "Objective-C Selectors in Xamarin.iOS"
description: "This document discusses how to interact with Objective-C selectors from C#. It describes how to invoke selectors and technical considerations that must be taken into account when doing so."
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/12/2017
---
# Objective-C selectors in Xamarin.iOS

The Objective-C language is based upon *selectors*. A selector is a
message that can be sent to an object or a *class*. [Xamarin.iOS](~/ios/internals/api-design/index.md) maps instance selectors
to instance methods, and class selectors to static methods.

Unlike normal C functions (and like C++ member functions), you cannot
directly invoke a selector using
[P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) Instead,
selectors are sent to an Objective-C class or instance using the
[`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
function.

For more information about messages in Objective-C, take a look at Apple's
[Working with Objects](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2)
guide.

## Example

Suppose you want to invoke the
[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
selector on [`NSString`](https://developer.apple.com/documentation/foundation/nsstring).
The declaration (from Apple's documentation) is:

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

This API has the following characteristics:

- The return type is `CGSize` for the Unified API.
- The `font` parameter is a [UIFont](xref:UIKit.UIFont) (and a type (indirectly) derived from [NSObject](xref:Foundation.NSObject), and is mapped to [System.IntPtr](xref:System.IntPtr).
- The `width` parameter, a `CGFloat`, is mapped to `nfloat`.
- The `lineBreakMode` parameter, a [`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc),
has already been bound in Xamarin.iOS as the
[`UILineBreakMode`](xref:UIKit.UILineBreakMode)
enumeration.

Putting it all together, the `objc_msgSend` declaration should match:

```csharp
CGSize objc_msgSend(
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Declare it as follows:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

To call this method, use code such as the following:

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle,
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

Had the returned value been a structure that was less than 8 bytes in size (like the older `SizeF` used before switching to the Unified APIs), the above code would have run on the simulator but crashed on the device. To call a selector that returns a value less than 8 bits in size, declare the `objc_msgSend_stret` function:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target,
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

To call this method, use code such as the following:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle,
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## Invoking a selector

Invoking a selector has three steps:

1. Get the selector target.
2. Get the selector name.
3. Call `objc_msgSend` with the appropriate arguments.

### Selector targets

A selector target is either an object instance or an Objective-C class. If
the target is an instance and came from a bound Xamarin.iOS type, use the [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle) property.

If the target is a class, use [`ObjCRuntime.Class`](xref:ObjCRuntime.Class) to get a reference to the class
instance, then use the [`Class.Handle`](xref:ObjCRuntime.Class.Handle) property.

### Selector names

Selector names are listed in Apple's documentation. For example, [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc) includes [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) and [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) selectors. The embedded and trailing colons are part of the selector name and cannot be omitted.

Once you have a selector name, you can create a [`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector) instance for it.

### Calling objc_msgSend

`objc_msgSend` sends a message (selector) to an object. This family of
functions takes at least two required arguments: the selector target (an
instance or class handle), the selector itself, and any arguments
required for the selector. The instance and selector arguments must be
`System.IntPtr`, and all remaining arguments must match the type the
selector expects, for example an `nint` for an `int`, or a
`System.IntPtr` for all `NSObject`-derived types. Use the
[`NSObject.Handle`](xref:Foundation.NSObject.Handle)
property to obtain an `IntPtr` for an Objective-C type instance.

There is more than one `objc_msgSend` function:

- Use [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
for selectors that return a struct. On ARM, this includes all return
types that are not an enumeration or any of the C built-in types (`char`,
`short`, `int`, `long`, `float`, `double`). On x86 (the simulator), this
method needs to be used for all structures larger than 8 bytes in size
(`CGSize` is 8 bytes and doesn't use `objc_msgSend_stret` in the
simulator).
- Use [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc)
for selectors that return a floating point value on x86 only. This
function does not need to be used on ARM; instead, use `objc_msgSend`.
- The main
[objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
function is used for all other selectors.

Once you've decided which `objc_msgSend` function(s) you need to call
(simulator and device may each require a different method), you can use
a normal [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute)
method to declare the function for later invocation.

A set of pre-made `objc_msgSend` declarations can be found in
`ObjCRuntime.Messaging`.

## Different invocations on simulator and device

As described above, Objective-C has three kinds of `objc_msgSend`
methods: one for regular invocations, one for invocations that return
floating point values (x86 only), and one for invocations that return
struct values. The latter includes the suffix `_stret` in
`ObjCRuntime.Messaging`.

If you are invoking a method that will return certain structs (rules
described below), you must invoke the method with the return value as the first
parameter as an `out` value:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

The rule for when to use the `_stret_` method differs on x86 and ARM.
If you want your bindings to work on both the simulator and the device,
add code such as the following:

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
}
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### Using the objc_msgSend_stret method

When building for ARM, use the
[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
for any value type that is not an enumeration or any of the base types
for an enumeration (`int`, `byte`, `short`, `long`, `double`, `float`).

When building for x86, use
[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
for any value type that is not an enumeration or any of the base types
for an enumeration (`int`, `byte`, `short`, `long`, `double`, `float`)
and whose native size is larger than 8 bytes.

### Creating your own signatures

The following [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) can be used to create your own signatures, if required.
