---
title: "Objective-C Selectors in Xamarin.iOS"
description: "This document discusses how to interact with Objective-C selectors from C#. It describes how to invoke selectors and technical considerations that must be taken into account when doing so."
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
---

# Objective-C Selectors in Xamarin.iOS

The Objective-C language is based upon *selectors*. A selector is a
message that can be sent to an object or a *class*. [Xamarin.iOS](~/ios/internals/api-design/index.md) maps instance selectors
to instance methods, and class selectors to static methods.

Unlike normal C functions (and like C++ member functions), you cannot
directly invoke a selector using [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Aside*: in theory you could use P/Invoke for non-virtual C++ member
functions, but you'd need to worry about per-compiler name mangling, which is a
world of pain better ignored.) Instead, selectors are sent to an Objective-C
class or instance using the [`objc_msgSend` function](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

You may find [this helpful guide on Objective-C messaging](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) useful.

<a name="Example" />

## Example

Suppose you want to invoke the [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) selector.
The declaration (from Apple's documentation) is:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  The return type is *CGSize* for the Unified API.
-  The  *font* parameter is a  [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (and a type (indirectly) derived from  [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ), and is thus mapped to  [System.IntPtr](xref:System.IntPtr) .
-  The  *width* parameter, a  *CGFloat* , is mapped to *nfloat*.
-  The  *lineBreakMode* parameter, a  *UILineBreakMode* , has already been bound in Xamarin.iOS as the  [UILineBreakMode enumeration](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Put it all together, and we want an objc_msgSend declaration that
matches:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

We will need to declare it:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Once declared, we can invoke it once we have the appropriate parameters:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

Had the returned value been a structure that was less than 8 bytes in size  (like the older `SizeF` used before switching to the Unified APIs) the above code would have run on the simulator but crashed on the device. So to call a Selector that returns a value less than 8 bits in size, consequently we *also*
need to declare the `objc_msgSend_stret()` function:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Invocation would then become:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## Invoking a Selector

Invoking a selector has three steps:

1.  Get the selector target.
1.  Get the selector name.
1.  Call objc_msgSend() with the appropriate arguments.


<a name="Selector_Targets" />

### Selector Targets

A selector target is either an object instance or an Objective-C class. If
the target is an instance and came from a bound Xamarin.iOS type, use the [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) property.

If the target is a class, use [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) to get a reference to the class
instance, then use the [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) property.


<a name="Selector_Names" />

### Selector Names

Selector names are listed within Apple's documentation. For example, the [UIKit NSString extension methods](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) include [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) and [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). The embedded and
trailing colons are important, and are part of the selector name.

Once you have a selector name, you can create a [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance for it.


<a name="Calling_objc_msgSend()" />

### Calling objc_msgSend()

 `objc_msgSend()` is used to send a message (selector) to an
object. This family of functions takes at least two required arguments: the
selector target (an instance or class handle), the selector itself, and then any
arguments required for the particular selector. The instance and selector
arguments must be `System.IntPtr`, and all remaining arguments must
match the type the selector expects, e.g. an `nint` for an `int`, or a `System.IntPtr` for all `NSObject`-derived types. Use the [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) property to obtain an `IntPtr`
for an Objective-C type instance.

Unfortunately, there is more than one `objc_msgSend()` function.

Use [`objc_msgSend_stret()`](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) for selectors which return a structure.
To keep things "interesting", on ARM this includes all return types that are *not* an enumeration or any of the C builtin types (char, short, int,
long, float, double). On x86 (the Simulator), this needs to be used for all
structures larger than 8 bytes in size. (CGSize -- used in the example above --
is 8 bytes exactly, and thus doesn't use `objc_msgSend_stret()` in
the simulator.)

Use [`objc_msgSend_fpret()`](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) for selectors which return a floating
point value on x86 only. This function does not need to be used on ARM; instead,
use `objc_msgSend()`.

The main [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) function is used for all other selectors.

Once you've decided which `objc_msgSend()` function(s) you need to
call (and it may be more than one, e.g. for simulator and device), you can use a
normal [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute) method to declare the function for later
invocation.

A set of pre-made `objc_msgSend()` declarations can be found in [`ObjCRuntime.Messaging`](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## The Ugly

Objective-C has three kinds of `objc_msgSend` methods: one for
regular invocations, one for invocations that return floating point values (x86
only), and one for invocations that return struct values. The latter includes
the suffix `_stret` in `ObjCRuntime.Messaging`.

If you are invoking a method that will return certain structures (rules are
described below), you must invoke the method with the return value as the first
parameter as an out value, like this:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Things get ugly here and because the rule for when you must use _stret_ is
different on X86 and ARM, if you want your bindings to work on both the
simulator and the device, you need to add code that looks like this:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### Using the objc\_msgSend\_stret method

The rule for when to use the [`objc_msgSend_stret`](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) are like this for **ARM**:

-  Any value type that is not an enumeration or any of the base types for an enumeration (int, byte, short, long, double, float).


The rule for when to use the [`objc_msgSend_stret`](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) are like this for **X86**:

-  Any value type that is not an enumeration, or any of the base types for an enumeration (int, byte, short, long, double, float) and whose native size is larger than 8 bytes.


### Creating your own signatures.

The following [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) can be used to create your own signatures, if required.



## Related Links

- [Selectors Sample](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
