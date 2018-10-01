---
title: "Limitations of Xamarin.iOS"
description: "This document describes the limitations of Xamarin.iOS, discussing generics, generic subclasses of NSObjects, P/Invokes in generic objects, and more."
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/09/2018
---

# Limitations of Xamarin.iOS

Since applications on the iPhone using Xamarin.iOS are compiled to static code,
	it is not possible to use any facilities that require code generation at
	runtime.

These are the Xamarin.iOS limitations compared to desktop Mono:

 <a name="Limited_Generics_Support" />


## Limited Generics Support

Unlike traditional Mono/.NET, code on the iPhone is statically compiled ahead
	of time instead of being compiled on demand by a JIT compiler.

Mono's [Full AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) technology has a few limitations
	with respect to generics, these are caused because not every
	possible generic instantiation can be determined up front at
	compile time. This is not a problem for regular .NET or Mono
	runtimes as the code is always compiled at runtime using the
	Just in Time compiler. But this poses a challenge for a static
	compiler like Xamarin.iOS.

Some of the common problems that developers run into, include:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### Generic Subclasses of NSObjects are limited

Xamarin.iOS currently has limited support for creating generic subclasses of the
	NSObject class, such as no support for generic methods. As of 7.2.1, using generic subclasses of NSObjects is possible, like this one:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> While generic subclasses of NSObjects are possible, there are a few limitations. Read the [Generic subclasses of NSObject](~/ios/internals/api-design/nsobject-generics.md) document for more information



### P/Invokes in Generic Types

P/Invokes in generic classes aren't supported:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### Property.SetInfo on a Nullable Type is not supported

Using Reflection's Property.SetInfo to set the value on a
	Nullable&lt;T&gt; is not currently supported.

 <a name="Value_types_as_Dictionary_Keys" />


### Value types as Dictionary Keys

Using a value type as a Dictionary&lt;TKey, TValue&gt; key
	is problematic, as the default Dictionary constructor attempts
	to use
	EqualityComparer&lt;TKey&gt;.Default. EqualityComparer&lt;TKey&gt;.Default,
	in turn, attempts to use Reflection to instantiate a new type
	which implements the IEqualityComparer&lt;TKey&gt;
	interface.

This works for reference types (as the reflection+create a
	new type step is skipped), but for value types it crashes and
	burns rather quickly once you attempt to use it on the
	device.

 **Workaround**: Manually implement
	the [IEqualityComparer&lt;TKey&gt;](xref:System.Collections.Generic.IEqualityComparer`1) 
	interface in a new type and provide an instance of that type
	to the [Dictionary&lt;TKey, TValue&gt;](xref:System.Collections.Generic.Dictionary`2) [(IEqualityComparer&lt;TKey&gt;)](xref:System.Collections.Generic.IEqualityComparer`1)
	constructor.


 <a name="No_Dynamic_Code_Generation" />


## No Dynamic Code Generation

Since the iPhone's kernel prevents an application from generating code
	dynamically Mono on the iPhone does not support any form of dynamic code
	generation. These include:

-  The System.Reflection.Emit is not available.
-  No support for System.Runtime.Remoting.
-  No support for creating types dynamically (no Type.GetType ("MyType`1")), although looking up existing types (Type.GetType ("System.String") for example, works just fine). 
-  Reverse callbacks must be registered with the runtime at compile time.


 
 <a name="System.Reflection.Emit" />


### System.Reflection.Emit

The lack of System.Reflection. **Emit** means that no code that
	depends on runtime code generation will work. This includes things like:

-  The Dynamic Language Runtime.
-  Any languages built on top of the Dynamic Language Runtime.
-  Remoting's TransparentProxy or anything else that would cause the runtime to generate code dynamically. 


 **Important:** Do not
	confuse **Reflection.Emit**
	with **Reflection**. Reflection.Emit is about
	generating code dynamically and have that code JITed and
	compiled to native code. Due to the limitations on the iPhone
	(no JIT compilation) this is not supported.

But the entire Reflection API, including Type.GetType
	("someClass"), listing methods, listing properties, fetching
	attributes and values works just fine.

### Using Delegates to call Native Functions

To call a native function through a C# delegate, the delegate's declaration 
must be decorated with one of the following attributes:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) 
  (preferred, since it is cross-platform and compatible with .NET Standard 
   1.1+)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Failing to provide one of these attributes will result in a runtime
error such as:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### Reverse Callbacks

In standard Mono it is possible to pass C# delegate
	instances to unmanaged code in lieu of a function pointer. The
	runtime would typically transform those function pointers into
	a small thunk that allows unmanaged code to call back into
	managed code.

In Mono these bridges are implemented by the Just-in-Time
	compiler. When using the ahead-of-time compiler required by
	the iPhone there are two important limitations at this
	point:

-  You must flag all of your callback methods with the 
   [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  The methods have to be static methods, there is no support for instance methods. 
 
<a name="No_Remoting" />

## No Remoting

The Remoting stack is not available on Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## Runtime Disabled Features

The following features have been disabled in Mono's iOS
	Runtime:

-  Profiler
-  Reflection.Emit
-  Reflection.Emit.Save functionality
-  COM bindings
-  The JIT engine
-  Metadata verifier (since there is no JIT)


 <a name=".NET_API_Limitations" />


## .NET API Limitations

The .NET API exposed is a subset of the full framework as not everything is
	available in iOS. See the FAQ for a [list of currently supported assemblies](~/cross-platform/internals/available-assemblies.md).



In particular, the API profile used by Xamarin.iOS does not
	include System.Configuration, so it is not possible to use
	external XML files to configure the behavior of the runtime.
