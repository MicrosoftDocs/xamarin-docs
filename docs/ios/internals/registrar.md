---
title: "Type Registrar for Xamarin.iOS"
description: "This document describes the Xamarin.iOS type registrar, which makes C# classes available to the Objective-C runtime."
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
---
# Type registrar for Xamarin.iOS

This document describes the type registration system used
by Xamarin.iOS.

## Registration of managed classes and methods

During startup, Xamarin.iOS will register:

- Classes with a [[Register]](xref:Foundation.RegisterAttribute) attribute as Objective-C classes.
- Classes with a [[Category]](xref:ObjCRuntime.CategoryAttribute) attribute as Objective-C categories.
- Interfaces with a [[Protocol]](xref:Foundation.ProtocolAttribute) attribute as Objective-C protocols.
- Members with an [[Export]](xref:Foundation.ExportAttribute), making it possible for Objective-C to access them.

For example, consider the managed `Main` method common in Xamarin.iOS
applications:

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

This code tells the Objective-C runtime to use the type called
`AppDelegate` as the application's delegate class. For the Objective-C
runtime to be able to create an instance of the C# `AppDelegate` class,
that class must be registered.

Xamarin.iOS performs registration automatically, either at
runtime (dynamic registration) or at compile time (static registration).

Dynamic registration uses reflection at startup to find all the
classes and methods to register, passing them to the Objective-C
runtime. Dynamic registration is used by default for simulator builds.

Static registration inspects, at compile time, the assemblies used
by the application. It determines the classes and methods to register
with Objective-C and generates a map, which is embedded into your binary.
Then, at startup, it registers the map with the Objective-C runtime. Static
registration is used for device builds.

### Categories

Starting with Xamarin.iOS 8.10, it is possible to create Objective-C
categories using C# syntax.

To create a category, use the `[Category]` attribute and specify the type to
extend. For example, the following code extends `NSString`:

```csharp
[Category (typeof (NSString))]
```

Each of a category's methods has an `[Export]` attribute, making it
available to the Objective-C runtime:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

All managed extension methods must be static, but it’s possible to create
Objective-C instance methods using the standard C# syntax for extension
methods:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

The first argument to the extension method is the instance on which the
method was invoked:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

This example will add a native `toUpper` instance method to
the `NSString` class. This method can be called from Objective-C:

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### Protocols

Starting with Xamarin.iOS 8.10, interfaces with the `[Protocol]`
attribute will be exported to Objective-C as protocols:

```csharp
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
```

This code exports `IMyProtocol` to Objective-C as a protocol called
`MyProtocol` and a class called `MyClass` that implements the protocol.

## New registration system

Starting with the stable 6.2.6 version and the beta 6.3.4
version, we've added a new static registrar. In the 7.2.1
version, we made the new registrar the default.

This new registration system offers the following new features:

- Compile-time detection of programmer errors:
  - Two classes being registered with the same name.
  - More than one method exported to respond to the same selector
- Removal of unused native code:
  - The new registration system will add strong references to code used in
    static libraries, allowing the native linker to strip out unused native
    code from the resulting binary. On Xamarin's sample bindings, most
    applications become at least 300k smaller.

- Support for generic subclasses of `NSObject`; see
  [NSObject Generics](~/ios/internals/api-design/nsobject-generics.md)
  for more information. Additionally the new registration system
  will catch unsupported generic constructs which would previously
  have caused random behavior at runtime.

### Errors caught by the new registrar

Below are some examples of the errors caught by the new registrar.

- Exporting the same selector more than once in the same class:

  ```csharp
  [Register]
  class MyDemo : NSObject
  {
      [Export ("foo:")]
      void Foo (NSString str);
      [Export ("foo:")]
      void Foo (string str)
  }
  ```

- Exporting more than one managed class with the same Objective-C name:

  ```csharp
  [Register ("Class")]
  class MyClass : NSObject {}

  [Register ("Class")]
  class YourClass : NSObject {}
  ```

- Exporting generic methods:

  ```csharp
  [Register]
  class MyDemo : NSObject
  {
      [Export ("foo")]
      void Foo<T> () {}
  }
  ```

### Limitations of the new registrar

Some things to keep in mind about the new registrar:

- Some third-party libraries must be updated to work with the new
  registration system. See [required modifications](#required-modifications)
  below for more details.

- A short-term downside is also that Clang must be used if the Accounts
  framework is used (this is because Apple's **accounts.h** header can only be
  compiled by Clang). Add `--compiler:clang` to the additional
  mtouch arguments to use Clang if you're using Xcode 4.6 or earlier
  (Xamarin.iOS will automatically select Clang in Xcode 5.0 or later.)

- If Xcode 4.6 (or earlier) is used, GCC/G++ must be
  selected if exported type names contain non-ASCII characters
  (this is because the version of Clang shipped with Xcode 4.6
  does not support non-ASCII characters inside identifiers in
  Objective-C code). Add `--compiler:gcc` to the
  additional mtouch arguments to use GCC.

## Selecting a registrar

You can select a different registrar by adding one of the
following options to the additional mtouch arguments in the
project's **iOS Build** settings:

- `--registrar:static` – default for device builds
- `--registrar:dynamic` – default for simulator builds

> [!NOTE]
> Xamarin's Classic API supported other options such as
> `--registrar:legacystatic` and `--registrar:legacydynamic`. However,
> these options are not supported by the Unified API.

## Shortcomings in the old registration system

The old registration system has the following drawbacks:

- There was no (native) static reference to Objective-C classes and methods in third-party native libraries, which meant that we couldn't ask the native linker to remove third-party native code that wasn't actually used (because everything would be removed). This is the reason for the `-force_load libNative.a` that every third-party binding had to do (or the equivalent `ForceLoad=true` in the
`[LinkWith]` attribute).
- You could export two managed types with the same Objective-C name with no warning. A rare scenario was to end up with two `AppDelegate` classes in different namespaces. At runtime it would be completely random which one was picked (in fact, it varied between runs of an app that wasn't even rebuilt - which made for a very puzzling and frustrating debugging experience).
- You could export two methods with the same Objective-C signature. Yet again which one would be called from Objective-C was random (but this problem wasn't as common as the previous one, mostly because the only way to actually experience this bug was to override the unlucky managed method).
- The set of methods that was exported was slightly different between dynamic and static builds.
- It does not work properly when exporting generic classes (which exact generic implementation executed at runtime would be random, effectively resulting in undetermined behavior).

<a name="required-modifications"></a>

## New registrar: required changes to bindings

This section describes bindings changes that must be made in order to
work with the new registrar.

### Protocols must have the [Protocol] attribute

Protocols must now have the `[Protocol]` attribute. If you do not do
this, you will a native linker error such as:

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### Selectors must have a valid number of parameters

All selectors must indicate number of parameters correctly. Previously,
these errors were ignored and could cause runtime problems.

In short, the number of colons must match the number of parameters:

- No parameters: `foo`
- One parameter: `foo:`
- Two parameters: `foo:parameterName2:`

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

Variadic functions must use the `IsVariadic` argument to the `[Export]`
attribute:

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### Must link to existing symbols

It is impossible to bind classes that don't exist in the native library.
If a class has been removed from or renamed in the native library, make
sure to update the bindings to match.
