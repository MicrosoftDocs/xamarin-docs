---
title: "Type Registrar"
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
---

# Type Registrar

This document describes the type registration system used
by Xamarin.iOS.

## Registration of managed classes and methods

During startup, Xamarin.iOS will register:

  - Classes with a [[Register]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) attribute as Objective-C classes.
  - Classes with a [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) attribute as Objective-C categories.
  - Interfaces with a [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) attribute as Objective-C protocols.

and in each cases members with an [[Export]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)
attribute are exported to the Objective-C. This allows managed classes to be created and managed
methods to be called from Objective-C and is the way that methods and
properties are linked between the C# world and the Objective-C one.

One very simple example is the AppDelegate class which
every application has. You will recall that the managed Main
method has a line like this one:

    UIApplication.Main (args, null, "AppDelegate");

This tells the Objective-C runtime to create the type
called "AppDelegate" as the application's delegate class.  For
the Objective-C runtime to know how to create an instance of
the "AppDelegate" class written in C#, this class has to be
registered.

Xamarin.iOS runtime will take care of the registration for
you, and internally, this registration can be done entirely at
runtime (dynamic registration) or it can be done at
compilation time (static registration).  The dynamic approach
involves using reflection at startup to find all the classes
and methods to register and pass those to the Objective-C
runtime.  The static approach inspects the assemblies used by
the application at compile time.  It determines the classes and
methods to register with Objective-C and generates a map
which is embedded into your binary.  Then, at startup, we
register the map with the Objective-C runtime.

### Categories

Starting with Xamarin.iOS 8.10 it will be possible to
create Objective-C categories using C# syntax.

This is done using the [Category] attribute, specifying
the type to extend as an argument to the attribute.
The following example will for instance extend NSString:

    [Category (typeof (NSString))]

Each category method is using the normal mechanism for
exporting methods to Objective-C using the [Export] attribute:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

All managed extension methods must be static, but it’s
possible to create Objective-C instance methods using
the standard syntax for extension methods in C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

and the first argument to the extension method will be
the instance on which the method was invoked.

Complete example:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

This example will add a native toUpper instance method to
the NSString class, which can be invoked from Objective-C.

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### Protocols

Starting with Xamarin.iOS 8.10 interfaces with the [Protocol]
attribute will be exported to Objective-C as protocols.

Example:

    [Protocol ("MyProtocol")]
    interface IMyProtocol
    {
        [Export ("method")]
        void Method ();
    }

    class MyClass : IMyProtocol
    {
        void Method ()
        {
        }
    }

This will be exported to Objective-C as a protocol (MyProtocol),
and a class (MyClass) which implements the protocol.

 **Dynamic registration**

is used for simulator builds,
as it accelerates the build/debug cycle.  This is the result
of eliminating the steps that generates the class mapping and
the compilation of this map table into your application
launcher every time you start your application, and instead a
general purpose launcher is used every time.  Your desktop
computer has plenty of power to perform the runtime scanning
of classes quickly, so performance is never an issue.

 **Static registration**

is intended for device builds as mobile
devices are slower than desktop computers, and performing
runtime scanning is slow.  Since device builds will
always need to create a new binary, the build/debug cycle is
not affected by the creation of the registration mapping.

## New Registration System

Starting with the stable 6.2.6 version and the beta 6.3.4
version we've added a new static registrar. In the 7.2.1
version we made the new registrar the default.

This new registration system offers the following new
	features:

- Compile time detection of programmer errors:
    - Two classes being registered with the same name.
    - More than one method exported to respond to the same selector



- Can remove unused native code
    - The new registration system will add strong references to code used in static libraries, allowing the native linker to strip out unused native code from the resulting binary.
      On Xamarin's sample bindings, most applications become at least 300k smaller.

- Support for generic subclasses of NSObject. See [NSObject Generics](~/ios/internals/api-design/nsobject-generics.md)
  for more information. Additionally the new registration system
  will catch unsupported generic constructs which would previously
  have caused random behavior at runtime.

These are some examples of the errors caught by the new
registar:

Exporting the same selector more than once in the same
class.

```csharp
[Register]
class MyDemo : NSObject {
	[Export ("foo:")]
	void Foo (NSString str);
	[Export ("foo:")]
	void Foo (string str)
}
```

Exporting more than one managed class with the same
Objective-C name.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Exporting generic methods.

```csharp
[Register]
class MyDemo : NSObject {
	[Export ("foo")]
	void Foo<T> () {}
}
```



Some things to keep in mind about the new registrar:
- Some third party libraries need to be updated to work with the new registration system, see the section [required modifications below](#required_modifications) for more details.
- A short term downside is also that Clang must be used if the Accounts framework is used (this is because Apple's accounts.h header can only
	be compiled by
	Clang). Add <code>--compiler:clang</code> to the
	additional mtouch arguments to use Clang if your're
	using Xcode 4.6 or earlier (Xamarin.iOS will automatically
	select Clang in Xcode 5.0 or later.)

	<li>If Xcode 4.6 (or earlier) is used, GCC/G++ must be
	selected if exported type names contain non-ascii characters
	(this is because the version of Clang shipped with Xcode 4.6
	does not support non-ascii characters inside identifiers in
	Objective-C code). Add <code>--compiler:gcc</code> to the
	additional mtouch arguments to use GCC.


## Selecting a registrar

You can select a different registrar by adding one of the
following options to the additional mtouch arguments in the
project's iOS Build options:

-  `--registrar:static` : default for device builds
-  `--registrar:dynamic` : default for simulator builds
-  `--registrar:legacystatic` : default for device builds until Xamarin.iOS 7.2.1
-  `--registrar:legacydynamic` : default for simulator builds until Xamarin.iOS 7.2.1


## Shortcomings in the Old Registration System

The old registration system has the following drawbacks:

-  There was no (native) static reference to Objective-C classes and methods in third-party native libraries, which meant that we couldn't ask the native linker to remove third-party native code that wasn't actually used (because everything would be removed). This is the reason for the "-force_load libNative.a" that every third-party binding had to do (or the equivalent ForceLoad=true in the [LinkWith] attribute).
-  You could export two managed types with the same Objective-C name, with no warning. A rare scenario was to end up with two AppDelegate classes (in different namespaces). At runtime it would be completely random which one was picked (in fact it varied between runs of an app that wasn't even rebuilt - which made for a very puzzling and frustrating debugging experience).
-  You could export two methods with the same Objective-C signature. Yet again which one would be called from Objective-C was random (but this problem wasn't as common as the previous one, mostly because the only way to actually experience this bug was to override the unlucky managed method).
-  The set of methods that was exported was slightly different between dynamic and static builds.
-  It does not work properly when exporting generic classes (which exact generic implementation executed at runtime would be random, effectively resulting in undetermined behavior).


 <a name="required_modifications" />


## New Registrar: Required Changes to Bindings


Existing Objective-C bindings might need to be updated to
work with the new registar.

Here is a list of changes that need to be done.

### Protocols must have the [Protocol] attribute

Protocols implementations must now have the [Protocol]
attribute applied to them.  If you do not do this, you will a
native linker error like this one:

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### Selectors must have a valid number of parameters

All selectors must indicate number of parameters
correctly.  Previously, these errors were ignored and could
cause runtime problems.

In short, the number of colons must match the number of parameters.

For example:

-  No parameters: `foo'
-  One parameter: `foo:'
-  Two parameters: `foo:parameterName2:'


The following are incorrect uses:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### Use IsVariadic parameter in Export

Variadic functions must say so in their Export attribute
(this is because the selector is wrong about the number of
arguments, i.e. point 2. from above is violated):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### Must link to existing symbols

It is impossible to bind classes that don't exist in the
native library.

You will get a native linker error if you try to bind non-existing
classes.

This usually happens when a binding has existed for some
time, and the native code has been modified during that time
so that a particular native class has either been removed or
renamed, while the binding has not been updated.

