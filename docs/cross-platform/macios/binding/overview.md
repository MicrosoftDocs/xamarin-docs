---
title: "Overview of Objective-C Bindings"
description: "This document provides an overview of different ways to create C# bindings for Objective-C code, including command-line bindings, binding projects, and Objective Sharpie. It also discusses how binding works."
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
---

# Overview of Objective-C Bindings

_Details of how the binding process works_

Binding an Objective-C library for use with Xamarin takes of three steps:

1. Write a C# "API definition" to describe how the native API is exposed in .NET, and how it
   maps to the underlying Objective-C. This is done using standard
   C# constructs like `interface` and various binding **attributes**
   (see this [simple example](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Once you have written the "API definition" in C#, you compile it
   to produce a "binding" assembly. This can be done on the [**command line**](#command-line-bindings) or
   using a [**binding project**](#bindingproject) in Visual Studio for Mac or Visual Studio.

3. That "binding" assembly is then added to your Xamarin application project,
   so you can access the native functionality using the API you defined.
   The binding project is completely separate from your application projects.

   > [!NOTE]
   > Step 1 can be automated with the assistance of
   > [**Objective Sharpie**](#objectivesharpie). It examines the Objective-C API
   > and generates a proposed C# "API definition." You can customize the files
   > created by Objective Sharpie and use them in a binding project
   > (or on the command line) to create your binding assembly. Objective Sharpie
   > does not create bindings by itself, it's merely an optional part of the larger process.

You can also read more technical details of [how it works](#howitworks), which will
help you to write your bindings.

## Command Line Bindings

You can use the `btouch-native` for Xamarin.iOS
(or `bmac-native` if you are using Xamarin.Mac) to build bindings directly. It
works by passing the C# API definitions that you've created by hand
(or using Objective Sharpie) to the command line tool (`btouch-native` for
iOS or `bmac-native` for Mac).

The general syntax for invoking these tools is:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

The above command will generate the file `cocos2d.dll` in the current
directory, and it will contain the fully bound library that you can use in your
project. This is the tool that Visual Studio for Mac uses to create your bindings if you use a
binding project (described [below](#bindingproject)).

<a name="bindingproject"></a>

## Binding Project

A binding project can be created in Visual Studio for Mac or Visual Studio (Visual Studio
only supports iOS bindings) and makes it easier to edit and build API
definitions for binding (versus using the command line).

Follow this [getting started guide](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)
to see how to create and use a binding project to produce a binding.

<a name="objectivesharpie"></a>

## Objective Sharpie

Objective Sharpie is another, separate command line tool that helps with the
initial stages of creating a binding. It does not create a binding by itself,
rather it automates the initial step of generating an API definition for the
target native library.

Read the [Objective Sharpie docs](~/cross-platform/macios/binding/objective-sharpie/index.md)
to learn how to parse native libraries, native frameworks, and CocoaPods into
API defintions that can be built into bindings.

<a name="howitworks"></a>

## How Binding Works

It is possible to use the [[Register]](xref:Foundation.RegisterAttribute) attribute,
[[Export]](xref:Foundation.ExportAttribute) attribute, and
[manual Objective-C selector invocation](~/ios/internals/objective-c-selectors.md)
together to manually bind new (previously unbound) Objective-C types.

First, find a type that you wish to bind. For discussion purposes (and
simplicity), we'll bind the [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) type (which has already been bound in [Foundation.NSEnumerator](xref:Foundation.NSEnumerator); the implementation below
is just for example purposes).

Second, we need to create the C# type. We'll likely want to place this into a
namespace; since Objective-C doesn't support namespaces, we'll need to use the
`[Register]` attribute to change the type name that Xamarin.iOS will register with
the Objective-C runtime. The C# type must also inherit from [Foundation.NSObject](xref:Foundation.NSObject):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Third, review the Objective-C documentation and create [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) instances for each selector
you wish to use. Place these within the class body:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Fourth, your type will need to provide constructors. You *must* chain
your constructor invocation to the base class constructor. The `[Export]`
attributes permit Objective-C code to call the constructors with the specified
selector name:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Fifth, provide methods for each of the Selectors declared in Step 3. These
will use `objc_msgSend()` to invoke the selector on the native object. Note the
use of [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) to convert an `IntPtr` into an
appropriately typed `NSObject` (sub-)type. If you want the method to be callable
from Objective-C code, the member *must* be **virtual**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Putting it all together:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```
