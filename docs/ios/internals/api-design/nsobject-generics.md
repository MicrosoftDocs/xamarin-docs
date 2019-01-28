---
title: "Generic subclasses of NSObject in Xamarin.iOS"
description: "This document describes how to create create generic subclasses of NSObject. It examines what can and cannot be done, discusses the static registrar, and takes a look at performance."
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
---

# Generic subclasses of NSObject in Xamarin.iOS

## Using generics with NSObjects

Starting with Xamarin.iOS 7.2.1 you can use generics in
subclasses of `NSObject` (for example [UIView](xref:UIKit.UIView).

You can now create generic classes, like this one:

```csharp
class Foo<T> : UIView {
	public Foo (CGRect x) : base (x) {}
	public override void Draw (CoreGraphics.CGRect rect)
	{
		Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
	}
}
```

Since objects that subclass `NSObject` are registered with
the Objective-C runtime there are some limitations as to what
is possible with generic subclasses of `NSObject` types.
	
## Considerations for generic subclasses of NSObject

This document details the limitations in the limited
support for generic subclasses of `NSObjects` introduced with
Xamarin.iOS 7.2.1.
	
### Generic Type Arguments in Member Signatures

All generic type arguments in a member signature exposed to
Objective-C must have an `NSObject` constraint.

**Good**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
	[Export ("myMethod:")]
	public void MyMethod (T value)
	{
	}
}
```

**Reason**: The generic type parameter is an `NSObject`, so
the selector signature for `myMethod:` can be safely exposed
to Objective-C (it will always be `NSObject` or a subclass of
it).

**Bad**:

```csharp
class Generic<T> : NSObject
{
	[Export ("myMethod:")]
	public void MyMethod (T value)
	{
	}
}
```

**Reason**: it is not possible to create an Objective-C
signature for the exported members that Objective-C code can
call, since the signature would differ depending on the exact
type of the generic type `T`.

**Good**:

```csharp
class Generic<T> : NSObject
{
	T storage;

	[Export ("myMethod:")]
	public void MyMethod (NSObject value)
	{
	}
}
```

**Reason**: it is possible to have unconstrained generic type
arguments as long as they do not take part of the exported
member signature.

**Good**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
	[Export ("myMethod:")]
	public void MyMethod (T value)
	{
		Console.WriteLine (typeof (U));
	}
}
```

**Reason**: the `T` parameter in the Objective-C exported
`MyMethod` is constrained to be an `NSObject`, the unconstrained
type `U` is not part of the signature.
	
### Instantiations of Generic Types from Objective-C

Instantiation of generic types from Objective-C is not
allowed. This typically occurs when a managed type is used in
a xib.

Consider this class definition, which exposes a constructor
that takes an `IntPtr` (the Xamarin.iOS way of constructing a C#
object from a native Objective-C instance):
	
```
class Generic<T> : NSObject where T : NSObject
{
	public Generic () {}
	public Generic (IntPtr ptr) : base (ptr) {}
}
```

While the above construct is fine, at runtime, this will
throw an exception at if Objective-C tries to create an
instance of it.

This is happens because Objective-C has no concept of
generic types, and it cannot  specify the exact generic type
to create.

This problem can be worked around by creating a specialized
subclass of the generic type.   For example:
	
```
class Generic<T> : NSObject where T : NSObject
{
	public Generic () {}
	public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Now there is no ambiguity anymore, the
class `GenericUIView` can be used in xibs.

## No support for generic methods

### Generic methods are not allowed.

The following code will not compile:

```csharp
class MyClass : NSObject
{
	[Export ("myMethod")]
	public void MyMethod<T> (T argument)
	{
	}
}
```

**Reason**: This is not allowed because Xamarin.iOS does not
know which type to use for the type argument `T` when the method
is invoked from Objective-C .

An alternative is to create a specialized method and export that instead:

```csharp
class MyClass : NSObject
{
	[Export ("myMethod")]
	public void MyUIViewMethod (UIView argument)
	{
		MyMethod<UIView> (argument);
	}
	public void MyMethod<T> (T argument)
	{
	}
}
```

### No exported static members allowed

You can not expose a static members to Objective-C if it is
hosted inside a generic subclass of `NSObject`.

Example of an unsupported scenario:

```csharp
class Generic<T> : NSObject where T : NSObject
{
	[Export ("myMethod:")]
	public static void MyMethod ()
	{
	}

	[Export ("myProperty")]
	public static T MyProperty { get; set; }
}
```

**Reason:** Just like generic methods, the Xamarin.iOS runtime
needs to be able to know what type to use for the generic type
argument T.

For instance members the instance itself is used (since
there will never be an instance Generic<T>, it will
always be Generic<SomeSpecificClass>), but for static
members this information is not present.

Note that this applies even if the member in question does
not use the type argument T in any way.

The alternative in this case is to create a specialized subclass:

```csharp
class GenericUIView : Generic<UIView>
{
	[Export ("myUIViewMethod")]
	public static void MyUIViewMethod ()
	{
		MyMethod ();
	}

	[Export ("myProperty")]
	public static UIView MyUIVIewProperty {
		get { return MyProperty; }
		set { MyProperty = value; }
	}
}

class Generic<T> : NSObject where T : NSObject
{
	public static void MyMethod () {}
	public static T MyProperty { get; set; }
}
```

### Requires new Static Registrar

The generics support requires the new [registration system](~/ios/internals/registrar.md).

If you try to use the old legacy registration system will
show warnings when it encounters generic types (in addition to not
generate correct code, resulting in undefined behavior).
	
## Performance

The static registrar can't resolve an exported member in a generic
type at build time as it usually does, it has to be looked up at
runtime. This means	that invoking such a method from Objective-C
is slightly slower than invoking members from non-generic classes.

