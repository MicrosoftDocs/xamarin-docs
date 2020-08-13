---
title: "Working With JNI and Xamarin.Android"
description: "Xamarin.Android permits writing Android apps with C# instead of Java. Several assemblies are provided with Xamarin.Android which provide bindings for Java libraries, including Mono.Android.dll and Mono.Android.GoogleMaps.dll. However, bindings are not provided for every possible Java library, and the bindings that are provided may not bind every Java type and member. To use unbound Java types and members, the Java Native Interface (JNI) may be used. This article illustrates how to use JNI to interact with Java types and members from Xamarin.Android applications."
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
---

# Working with JNI and Xamarin.Android

_Xamarin.Android permits writing Android apps with C# instead of Java. Several assemblies are provided with Xamarin.Android which provide bindings for Java libraries, including Mono.Android.dll and Mono.Android.GoogleMaps.dll. However, bindings are not provided for every possible Java library, and the bindings that are provided may not bind every Java type and member. To use unbound Java types and members, the Java Native Interface (JNI) may be used. This article illustrates how to use JNI to interact with Java types and members from Xamarin.Android applications._

## Overview

It is not always necessary or possible to create a Managed Callable
Wrapper (MCW) to invoke Java code. In many cases, "inline" JNI is
perfectly acceptable and useful for one-off use of unbound Java
members. It is often simpler to use JNI to invoke a single method on a
Java class than to generate an entire .jar binding.

Xamarin.Android provides the `Mono.Android.dll` assembly, which
provides a binding for Android's `android.jar` library. Types and
members not present within `Mono.Android.dll` and types not present
within `android.jar` may be used by manually binding them. To bind Java
types and members, you use the **Java Native Interface** (**JNI**) to
lookup types, read and write fields, and invoke methods.

The JNI API in Xamarin.Android is conceptually very similar to the
`System.Reflection` API in .NET: it makes it possible for you to look
up types and members by name, read and write field values, invoke
methods, and more. You can use JNI and the
`Android.Runtime.RegisterAttribute` custom attribute to declare virtual
methods that can be bound to support overriding. You can bind
interfaces so that they can be implemented in C#.

This document explains:

- How JNI refers to types.
- How to lookup, read, and write fields.
- How to lookup and invoke methods.
- How to expose virtual methods to allow overriding from managed code.
- How to expose interfaces.

## Requirements

JNI, as exposed through the
[Android.Runtime.JNIEnv namespace](xref:Android.Runtime.JNIEnv),
is available in every version of Xamarin.Android.
To bind Java types and interfaces, you must use Xamarin.Android 4.0 or later.

## Managed Callable Wrappers

A **Managed Callable Wrapper** (**MCW**) is a *binding* for a Java
class or interface which wraps up the all the JNI machinery so that
client C# code doesn't need to worry about the underlying complexity of
JNI. Most of `Mono.Android.dll` consists of managed callable wrappers.

Managed callable wrappers serve two purposes:

1. Encapsulate JNI use so that client code doesn't need to know about the underlying complexity.
1. Make it possible to sub-class Java types and implement Java interfaces.

The first purpose is purely for convenience and encapsulation of
complexity so that consumers have a simple, managed set of classes to
use. This requires use of the various
[JNIEnv](xref:Android.Runtime.JNIEnv)
members as described later in this article. Keep in mind that managed
callable wrappers aren't strictly necessary &ndash; "inline" JNI use is
perfectly acceptable and is useful for one-off use of unbound Java
members. Sub-classing and interface implementation requires the use of
managed callable wrappers.

## Android Callable Wrappers

Android callable wrappers (ACW) are required whenever the Android
runtime (ART) needs to invoke managed code; these wrappers are required
because there is no way to register classes with ART at runtime.
(Specifically, the
[DefineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986)
JNI function is not supported by the Android runtime. Android callable
wrappers thus make up for the lack of runtime type registration
support.)

Whenever Android code needs to execute a virtual or interface method
that is overridden or implemented in managed code, Xamarin.Android
must provide a Java proxy so that this method gets dispatched to the
appropriate managed type. These Java proxy types are Java code that
have the "same" base class and Java interface list as the managed type,
implementing the same constructors and declaring any overridden base
class and interface methods.

Android callable wrappers are generated by the **monodroid.exe** program
during the
[build process](~/android/deploy-test/building-apps/build-process.md), and are
generated for all types that (directly or indirectly) inherit
[Java.Lang.Object](xref:Java.Lang.Object).

### Implementing Interfaces

There are times when you may need to implement an Android interface,
(such as [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks)).

All Android classes and interfaces extend the
[Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject)
interface; therefore, all Android types must implement `IJavaObject`.
Xamarin.Android takes advantage of this fact &ndash; it uses
`IJavaObject` to provide Android with a Java proxy (an Android callable
wrapper) for the given managed type. Because **monodroid.exe** only
looks for `Java.Lang.Object` subclasses (which must implement
`IJavaObject`), subclassing `Java.Lang.Object` provides us with a way
to implement interfaces in managed code. For example:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```

### Implementation Details

*The remainder of this article provides implementation details subject
to change without notice* (and is presented here only because
developers may be curious about what's going on under the hood).

For example, given the following C# source:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

The **mandroid.exe** program will generate the following Android Callable
Wrapper:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Notice that the base class is preserved, and native method declarations are
provided for each method that is overridden within managed code.

### ExportAttribute and ExportFieldAttribute

Typically, Xamarin.Android automatically generates the Java code that
comprises the ACW; this generation is based on the class and method names when a class
derives from a Java class and overrides existing Java methods. However,
in some scenarios, the code generation is not adequate, as outlined
below:

- Android supports action names in layout XML attributes, for example
    the [android:onClick](xref:Android.Views.View.IOnClickListener.OnClick*)
    XML attribute. When it is specified, the inflated View instance tries
    to look up the Java method.

- The [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html)
    interface requires `readObject` and `writeObject` methods. Since
    they are not members of this interface, our corresponding managed
    implementation does not expose these methods to Java code.

- The [android.os.Parcelable](xref:Android.OS.Parcelable)
    interface expects that an implementation class must have a static
    field `CREATOR` of type `Parcelable.Creator`. The generated Java code
    requires some explicit field. With our standard scenario, there is
    no way to output field in Java code from managed code.

Because code generation does not provide a solution to generate
arbitrary Java methods with arbitrary names, starting with
Xamarin.Android 4.2, the
[ExportAttribute](xref:Java.Interop.ExportAttribute) and
[ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute)
were introduced to offer a solution to the above scenarios. Both
attributes reside in the `Java.Interop` namespace:

- `ExportAttribute` &ndash; specifies a method name and its expected
    exception types (to give explicit "throws" in Java). When it
    is used on a method, the method will "export" a Java method that
    generates a dispatch code to the corresponding JNI invocation to
    the managed method. This can be used with `android:onClick` and
    `java.io.Serializable`.

- `ExportFieldAttribute` &ndash; specifies a field name. It resides
    on a method that works as a field initializer. This can be used with
    `android.os.Parcelable`.

#### Troubleshooting ExportAttribute and ExportFieldAttribute

- Packaging fails due to missing **Mono.Android.Export.dll** &ndash;
    if you used `ExportAttribute` or `ExportFieldAttribute` on some methods
    in your code or dependent libraries, you have to add
    **Mono.Android.Export.dll**. This assembly is isolated to support
    callback code from Java. It is separate from **Mono.Android.dll** as it
    adds additional size to the application.

- In Release build, `MissingMethodException` occurs for Export methods
    &ndash; In Release build, `MissingMethodException` occurs for
    Export methods. (This issue is fixed in the latest version of Xamarin.Android.)

### ExportParameterAttribute

`ExportAttribute` and `ExportFieldAttribute` provide functionality that
Java run-time code can use. This run-time code accesses managed code
through the generated JNI methods driven by those attributes. As a
result, there is no existing Java method that the managed method binds;
hence, the Java method is generated from a managed method signature.

However, this case is not fully determinant. Most notably, this is true
in some advanced mappings between managed types and Java types such as:

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

When types such as these are needed for exported methods, the
`ExportParameterAttribute` must be used to explicitly give the corresponding
parameter or return value a type.

### Annotation Attribute

In Xamarin.Android 4.2, we converted `IAnnotation` implementation types into
attributes (System.Attribute), and added support for annotation generation in
Java wrappers.

This means the following directional changes:

- The binding generator generates `Java.Lang.DeprecatedAttribute`
    from `java.Lang.Deprecated` (while it should be `[Obsolete]` in
    managed code).

- This does not mean that existing `Java.Lang.Deprecated` class will
    vanish. These Java-based objects could be still used as usual Java
    objects (if such usage exists). There will be `Deprecated` and
    `DeprecatedAttribute` classes.

- The `Java.Lang.DeprecatedAttribute` class is marked as
    `[Annotation]` . When there is a custom attribute that is inherited
    from this `[Annotation]` attribute, msbuild task will generate a
    Java annotation for that custom attribute (@Deprecated) in the
    Android Callable Wrapper (ACW).

- Annotations could be generated onto classes, methods and exported
    fields (which is a method in managed code).

If the containing class (the annotated class itself, or the class that
contains the annotated members) is not registered, the entire Java class source
is not generated at all, including annotations. For methods, you can specify the
`ExportAttribute` to get the method explicitly generated and annotated. Also,
it is not a feature to "generate" a Java annotation class definition. In
other words, if you define a custom managed attribute for a certain annotation,
you'll have to add another .jar library that contains the corresponding Java
annotation class. Adding a Java source file that defines the annotation type is
not sufficient. The Java compiler does not work in the same way as **apt**.

Additionally the following limitations apply:

- This conversion process does not consider `@Target` annotation on
    the annotation type so far.

- Attributes onto a property does not work. Use attributes for
    property getter or setter instead.

## Class Binding

Binding a class means writing a managed callable wrapper to simplify
invocation of the underlying Java type.

Binding virtual and abstract methods to permit overriding from C# requires
Xamarin.Android 4.0. However, any version of Xamarin.Android can bind
non-virtual methods, static methods, or virtual methods without supporting
overrides.

A binding typically contains the following items:

- A [JNI handle to the Java type being bound](#_Looking_up_Java_Types).

- [JNI field IDs and properties for each bound field](#_Instance_Fields).

- [JNI method IDs and methods for each bound method](#_Instance_Methods).

- If sub-classing is required, the type needs to have a
   [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
   custom attribute on the type declaration with
   [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) set to `true`.

### Declaring Type Handle

The field and method lookup methods require an object reference
referring to their declaring type. By convention, this is held in a
`class_ref` field:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

See the [JNI Type References](#_JNI_Type_References) section for details about
the `CLASS` token.

### Binding Fields

Java fields are exposed as C# properties, for example the Java field
[java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in)
is bound as the C# property
[Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In).
Furthermore, since JNI distinguishes between static fields and instance
fields, different methods be used when implementing the properties.

Field binding involves three sets of methods:

1. The *get field id* method. The *get field id* method is responsible
    for returning a field handle that the *get field value* and *set
    field value* methods will use. Obtaining the field id requires
    knowing the declaring type, the name of the field, and the
    [JNI type signature](#JNI_Type_Signatures) of the field.

1. The *get field value* methods. These methods require the field
    handle and are responsible for reading the field's value from Java.
    The method to use depends upon the field's type.

1. The *set field value* methods. These methods require the field
    handle and are responsible for writing the field's value within
    Java. The method to use depends upon the field's type.

[Static fields](#_Static_Fields) use the [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field`, and [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) methods.

 [Instance fields](#_Instance_Fields) use the [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field`, and [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) methods.

For example, the static property `JavaSystem.In` can be
implemented as:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Note: We're using [InputStreamInvoker.FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) to convert the JNI
reference into a `System.IO.Stream` instance, and we're using `JniHandleOwnership.TransferLocalRef` because [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) returns a local reference.

Many of the [Android.Runtime](xref:Android.Runtime) types have `FromJniHandle`
methods which will convert a JNI reference into the desired type.

### Method Binding

Java methods are exposed as C# methods and as C# properties. For
example, the Java method
[java.lang.Runtime.runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean))
method is bound as the
[Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*)
method, and the
[java.lang.Object.getClass](https://developer.android.com/reference/java/lang/Object.html#getClass)
method is bound as the
[Java.Lang.Object.Class](xref:Java.Lang.Object.Class)
property.

Method invocation is a two-step process:

1. The  *get method id* for the method to invoke. The  *get method id* method is responsible for returning a method handle that the method invocation methods will use. Obtaining the method id requires knowing the declaring type, the name of the method, and the  [JNI type signature](#JNI_Type_Signatures) of the method.

1. Invoke the method.

Just as with fields, the methods to use to get the method id and invoke the
method differ between static methods and instance methods.

[Static methods](#_Static_Methods_1) use
[JNIEnv.GetStaticMethodID()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)
to lookup the method id, and use the `JNIEnv.CallStatic*Method` family
of methods for invocation.

[Instance methods](#_Instance_Methods) use
[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)
to lookup the method id, and use the `JNIEnv.Call*Method` and
`JNIEnv.CallNonvirtual*Method` families of methods for invocation.

Method binding is potentially more than just method invocation. Method
binding also includes allowing a method to be overridden (for abstract
and non-final methods) or implemented (for interface methods). The
[Supporting Inheritance, Interfaces](#_Supporting_Inheritance,_Interfaces_1) section covers the
complexities of supporting virtual methods and interface methods.

<a name="_Static_Methods_1"></a>

#### Static Methods

Binding a static method involves using `JNIEnv.GetStaticMethodID` to
obtain a method handle, then using the appropriate
`JNIEnv.CallStatic*Method` method, depending on the method's return
type. The following is an example of a binding for the
[Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime())
method:

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Note that we store the method handle in a static field,
`id_getRuntime`. This is a performance optimization, so that the method
handle doesn't need to be looked up on every invocation. It is not
necessary to cache the method handle in this way. Once the method
handle is obtained,
[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)
is used to invoke the method. `JNIEnv.CallStaticObjectMethod` returns
an `IntPtr` which contains the handle of the returned Java instance.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*)
is used to convert the Java handle into a strongly typed object
instance.

#### Non-virtual Instance Method Binding

Binding a `final` instance method, or an instance method which doesn't
require overriding, involves using `JNIEnv.GetMethodID` to obtain a
method handle, then using the appropriate `JNIEnv.Call*Method`
method, depending on the method's return type. The following is an
example of a binding for the `Object.Class` property:

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Note that we store the method handle in a static field, `id_getClass`.
This is a performance optimization, so that the method handle doesn't
need to be looked up on every invocation. It is not necessary to cache
the method handle in this way. Once the method handle is obtained,
[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)
is used to invoke the method. `JNIEnv.CallStaticObjectMethod` returns
an `IntPtr` which contains the handle of the returned Java instance.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*)
is used to convert the Java handle into a strongly typed object
instance.

### Binding Constructors

Constructors are Java methods with the name `"<init>"`. Just as with
Java instance methods, `JNIEnv.GetMethodID` is used to lookup the
constructor handle. Unlike Java methods, the
[JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*)
methods are used to invoke the constructor method handle. The return
value of `JNIEnv.NewObject` is a JNI local reference:

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normally a class binding will subclass
[Java.Lang.Object](xref:Java.Lang.Object).
When subclassing `Java.Lang.Object`, an additional semantic comes into
play: a `Java.Lang.Object` instance maintains a global reference to a
Java instance through the `Java.Lang.Object.Handle` property.

1. The `Java.Lang.Object` default constructor will allocate a Java
    instance.

1. If the type has a `RegisterAttribute` , and
    `RegisterAttribute.DoNotGenerateAcw` is `true` , then an instance
    of the `RegisterAttribute.Name` type is created through its default
    constructor.

1. Otherwise, the
    [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md)
    (ACW) corresponding to `this.GetType` is instantiated
    through its default constructor. Android Callable Wrappers are
    generated during package creation for every `Java.Lang.Object`
    subclass for which `RegisterAttribute.DoNotGenerateAcw` is not set
    to `true`.

For types which are not class bindings, this is the expected semantic:
instantiating a `Mono.Samples.HelloWorld.HelloAndroid` C# instance
should construct a Java `mono.samples.helloworld.HelloAndroid` instance
which is a generated Android Callable Wrapper.

For class bindings, this may be the correct behavior if the Java type
contains a default constructor and/or no other constructor needs to be
invoked. Otherwise, a constructor must be provided which performs the
following actions:

1. Invoking the [Java.Lang.Object(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object#ctor*)
    instead of the default `Java.Lang.Object` constructor. This is
    needed to avoid creating a new Java instance.

1. Check the value of [Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle)
    before creating any Java instances. The `Object.Handle` property
    will have a value other than `IntPtr.Zero` if an Android Callable
    Wrapper was constructed in Java code, and the class binding is
    being constructed to contain the created Android Callable Wrapper
    instance. For example, when Android creates a
    `mono.samples.helloworld.HelloAndroid` instance, the Android
    Callable Wrapper will be created first , and the Java
    `HelloAndroid` constructor will create an instance of the
    corresponding `Mono.Samples.HelloWorld.HelloAndroid` type, with the
    `Object.Handle` property being set to the Java instance prior to
    constructor execution.

1. If the current runtime type is not the same as the declaring type,
    then an instance of the corresponding Android Callable Wrapper must
    be created, and use [Object.SetHandle](xref:Java.Lang.Object.SetHandle*)
    to store the handle returned by
    [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*).

1. If the current runtime type is the same as the declaring type, then
    invoke the Java constructor and use
    [Object.SetHandle](xref:Java.Lang.Object.SetHandle*)
    to store the handle returned by `JNIEnv.NewInstance` .

For example, consider the
[java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int))
constructor. This is bound as:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

The [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*)
methods are helpers to perform a `JNIEnv.FindClass`,
`JNIEnv.GetMethodID`, `JNIEnv.NewObject`, and
`JNIEnv.DeleteGlobalReference` on the value returned from
`JNIEnv.FindClass`. See the next section for details.

<a name="_Supporting_Inheritance,_Interfaces_1"></a>

### Supporting Inheritance, Interfaces

Subclassing a Java type or implementing a Java interface requires the
generation of
[Android Callable Wrappers](~/android/platform/java-integration/android-callable-wrappers.md)
(ACWs) that are generated for every `Java.Lang.Object` subclass
during the packaging process. ACW generation is controlled through the
[Android.Runtime.RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
custom attribute.

For C# types, the `[Register]` custom attribute constructor requires
one argument: the [JNI simplified type reference](#_Simplified_Type_References_1) for the
corresponding Java type. This allows providing different names between
Java and C#.

Prior to Xamarin.Android 4.0, the `[Register]` custom attribute was
unavailable to "alias" existing Java types. This is because the ACW
generation process would generate ACWs for every `Java.Lang.Object`
subclass encountered.

Xamarin.Android 4.0 introduced the
[RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) property. This property instructs the ACW generation process to *skip*
the annotated type, allowing the declaration of new Managed Callable
Wrappers that will not result in ACWs being generated at package
creation time. This allows binding existing Java types. For instance,
consider the following simple Java class, `Adder`, which contains one
method, `add`, that adds to integers and returns the result:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

The `Adder` type could be bound as:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Here, the `Adder` C# type *aliases* the `Adder` Java type. The
`[Register]` attribute is used to specify the JNI name of the
`mono.android.test.Adder` Java type, and the `DoNotGenerateAcw`
property is used to inhibit ACW generation. This will result in the
generation of an ACW for the `ManagedAdder` type, which properly
subclasses the `mono.android.test.Adder` type. If the
`RegisterAttribute.DoNotGenerateAcw` property hadn't been used, then
the Xamarin.Android build process would have generated a new
`mono.android.test.Adder` Java type. This would result in compilation
errors, as the `mono.android.test.Adder` type would be present twice,
in two separate files.

### Binding Virtual Methods

`ManagedAdder` subclasses the Java `Adder` type, but it isn't
particularly interesting: the C# `Adder` type doesn't define any
virtual methods, so `ManagedAdder` can't override anything.

Binding `virtual` methods to permit overriding by subclasses requires
several things that need to be done which fall into the following two
categories:

1. **Method Binding**

1. **Method Registration**

#### Method Binding

A method binding requires the addition of two support members to the C#
`Adder` definition: `ThresholdType`, and `ThresholdClass`.

##### ThresholdType

The `ThresholdType` property returns the current type of the binding:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` is used in the Method Binding to determine when it
should perform virtual vs. non-virtual method dispatch. It should
always return a `System.Type` instance which corresponds to the
declaring C# type.

##### ThresholdClass

The `ThresholdClass` property returns the JNI class reference for the
bound type:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` is used in the Method Binding when invoking
non-virtual methods.

#### Binding Implementation

The method binding implementation is responsible for runtime invocation
of the Java method. It also contains a `[Register]` custom attribute
declaration that is part of the method registration, and will be
discussed in the Method Registration section:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

The `id_add` field contains the method ID for the Java method to
invoke. The `id_add` value is obtained from `JNIEnv.GetMethodID`,
which requires the declaring class (`class_ref`), the Java method name
(`"add"`), and the JNI signature of the method (`"(II)I"`).

Once the method ID is obtained, `GetType` is compared to
`ThresholdType` to determine if virtual or non-virtual dispatch is
required. Virtual dispatch is required when `GetType` matches
`ThresholdType`, as `Handle` may refer to a Java-allocated subclass
which overrides the method.

When `GetType` doesn't match `ThresholdType`, `Adder` has been
subclassed (e.g. by `ManagedAdder`), and the `Adder.Add`
implementation will only be invoked if the subclass invoked
`base.Add`. This is the non-virtual dispatch case, which is where
`ThresholdClass` comes in. `ThresholdClass` specifies which Java class
will provide the implementation of the method to invoke.

#### Method Registration

Assume we have an updated `ManagedAdder` definition which overrides the
`Adder.Add` method:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Recall that `Adder.Add` had a `[Register]` custom attribute:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

The `[Register]` custom attribute constructor accepts three
values:

1. The name of the Java method, `"add"` in this case.

1. The JNI Type Signature of the method, `"(II)I"` in this case.

1. The *connector method* , `GetAddHandler` in this case.
    Connector methods will be discussed later.

The first two parameters allow the ACW generation process to generate a
method declaration to override the method. The resulting ACW would
contain some of the following code:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Note that an `@Override` method is declared, which delegates to an
`n_`-prefixed method of the same name. This ensure that when Java code
invokes `ManagedAdder.add`, `ManagedAdder.n_add` will be invoked,
which will allow the overriding C# `ManagedAdder.Add` method to be
executed.

Thus, the most important question: how is `ManagedAdder.n_add`
hooked up to `ManagedAdder.Add`?

Java `native` methods are registered with the Java (the Android
runtime) runtime through the
[JNI RegisterNatives function](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` takes an array of structures containing the Java
method name, the JNI Type Signature, and a function pointer to invoke
that follows
[JNI calling convention](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
The function pointer must be a function that takes two pointer
arguments followed by the method parameters. The Java
`ManagedAdder.n_add` method must be implemented through a function
that has the following C prototype:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android does not expose a `RegisterNatives` method. Instead,
the ACW and the MCW together provide the information necessary to
invoke `RegisterNatives`: the ACW contains the method name and the
JNI type signature, the only thing missing is a function pointer to
hook up.

This is where the *connector method* comes in. The third `[Register]`
custom attribute parameter is the name of a method defined in the
registered type or a base class of the registered type that accepts no
parameters and returns a `System.Delegate`. The returned
`System.Delegate` in turn refers to a method that has the correct JNI
function signature. Finally, the delegate that the connector method
returns *must* be rooted so that the GC doesn't collect it, as the
delegate is being provided to Java.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

The `GetAddHandler` method creates a `Func<IntPtr, IntPtr, int, int,
int>` delegate which refers to the `n_Add` method, then invokes
[JNINativeWrapper.CreateDelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*).
`JNINativeWrapper.CreateDelegate` wraps the provided method in a
try/catch block, so that any unhandled exceptions are handled and will
result in raising the [AndroidEvent.UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser)
event. The resulting delegate is stored in the static `cb_add` variable
so that the GC will not free the delegate.

Finally, the `n_Add` method is responsible for marshaling the
JNI parameters to the corresponding managed types, then delegating the method
call.

Note: Always use `JniHandleOwnership.DoNotTransfer` when obtaining an MCW over
a Java instance. Treating them as a local reference (and thus calling
`JNIEnv.DeleteLocalRef`) will break managed -&gt; Java -&gt; managed stack
transitions.

### Complete Adder Binding

The complete managed binding for the `mono.android.tests.Adder` type
is:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```

### Restrictions

When writing a type that matches the following criteria:

1. Subclasses `Java.Lang.Object`

1. Has a `[Register]` custom attribute

1. `RegisterAttribute.DoNotGenerateAcw` is `true`

Then for GC interaction the type *must not* have any fields which may
refer to a `Java.Lang.Object` or `Java.Lang.Object` subclass at
runtime. For example, fields of type `System.Object` and any interface
type are not permitted. Types which cannot refer to `Java.Lang.Object`
instances are permitted, such as `System.String` and `List<int>`. This
restriction is to prevent premature object collection by the GC.

If the type must contain an instance field that can refer to a
`Java.Lang.Object` instance, then the field type must be
`System.WeakReference` or `GCHandle`.

## Binding Abstract Methods

Binding `abstract` methods is largely identical to binding virtual
methods. There are only two differences:

1. The abstract method is abstract. It still retains the `[Register]`
    attribute and the associated Method Registration, the Method
    Binding is just moved to the `Invoker` type.

1. A non- `abstract` `Invoker` type is created which subclasses the
    abstract type. The `Invoker` type must override all abstract
    methods declared in the base class, and the overridden
    implementation is the Method Binding implementation, though the
    non-virtual dispatch case can be ignored.

For example, assume that the above `mono.android.test.Adder.add` method
were `abstract`. The C# binding would change so that `Adder.Add` were
abstract, and a new `AdderInvoker` type would be defined which
implemented `Adder.Add`:

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

The `Invoker` type is only necessary when obtaining JNI references to
Java-created instances.

## Binding Interfaces

Binding interfaces is conceptually similar to binding classes
containing virtual methods, but many of the specifics differ in subtle
(and not so subtle) ways. Consider the following
[Java interface declaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Interface bindings have two parts: the C# interface definition, and an
Invoker definition for the interface.

### Interface Definition

The C# interface definition must fulfill the following requirements:

- The interface definition must have a `[Register]` custom attribute.

- The interface definition must extend the `IJavaObject interface`.
    Failure to do so will prevent ACWs from inheriting from the Java
    interface.

- Each interface method must contain a `[Register]` attribute
    specifying the corresponding Java method name, the JNI signature,
    and the connector method.

- The connector method must also specify the type that the connector
    method can be located on.

When binding `abstract` and `virtual` methods, the connector method
would be searched within the inheritance hierarchy of the type being
registered. Interfaces can have no methods containing bodies, so this
doesn't work, thus the requirement that a type be specified indicating
where the connector method is located. The type is specified within the
connector method string, after a colon `':'`, and must be the assembly
qualified type name of the type containing the invoker.

Interface method declarations are a translation of the corresponding
Java method using *compatible* types. For Java builtin types, the
compatible types are the corresponding C# types, e.g. Java `int` is C#
`int`. For reference types, the compatible type is a type that can
provide a JNI handle of the appropriate Java type.

The interface members will not be directly invoked by Java &ndash;
invocation will be mediated through the Invoker type &ndash; so some
amount of flexibility is permitted.

The Java Progress interface can be
[declared in C# as](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Notice in the above that we map the Java `int[]` parameter to a
[JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1).
This isn't necessary: we could have bound it to a C# `int[]`, or an `IList<int>`,
or something else entirely. Whatever type is chosen, the `Invoker` needs to be
able to translate it into a Java `int[]` type for
invocation.

### Invoker Definition

The `Invoker` type definition must inherit `Java.Lang.Object`,
implement the appropriate interface, and provide all connection methods
referenced in the interface definition. There is one more suggestion
that differs from a class binding: the `class_ref` field and method IDs
should be instance members, not static members.

The reason for preferring instance members has to do with
`JNIEnv.GetMethodID` behavior in the Android runtime. (This may be
Java behavior as well; it hasn't been tested.) `JNIEnv.GetMethodID`
returns null when looking up a method that comes from an implemented
interface and not the declared interface. Consider the
[java.util.SortedMap&lt;K, V&gt;](https://developer.android.com/reference/java/util/SortedMap.html)
Java interface, which implements the
[java.util.Map&lt;K, V&gt;](https://developer.android.com/reference/java/util/Map.html)
interface. Map provides a
[clear](https://developer.android.com/reference/java/util/Map.html#clear())
method, thus a seemingly reasonable `Invoker` definition for SortedMap
would be:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

The above will fail because `JNIEnv.GetMethodID` will return `null`
when looking up the `Map.clear` method through the `SortedMap` class
instance.

There are two solutions to this: track which interface every method
comes from, and have a `class_ref` for each interface, or keep
everything as instance members and perform the method lookup on the
most-derived class type, not the interface type. The latter is done in
**Mono.Android.dll**.

The Invoker definition has six sections: the constructor, the `Dispose`
method, the `ThresholdType` and `ThresholdClass` members, the
`GetObject` method, interface method implementation, and the connector
method implementation.

#### Constructor

The constructor needs to lookup the runtime class of the instance being
invoked and store the runtime class in the instance `class_ref` field:

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Note: The `Handle` property must be used within the constructor body,
and not the `handle` parameter, as on Android v4.0 the `handle`
parameter may be invalid after the base constructor finishes executing.

#### Dispose Method

The `Dispose` method needs to free the global reference allocated in
the constructor:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```

#### ThresholdType and ThresholdClass

The `ThresholdType` and `ThresholdClass` members are identical to what
is found in a class binding:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

#### GetObject Method

A static `GetObject` method is required to support
[Extensions.JavaCast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### Interface Methods

Every method of the interface needs to have an implementation, which
invokes the corresponding Java method through JNI:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```

#### Connector Methods

The connector methods and supporting infrastructure are responsible for
marshaling the JNI parameters to appropriate C# types. The Java `int[]`
parameter will be passed as a JNI `jintArray`, which is an `IntPtr`
within C#. The `IntPtr` must be marshaled to a `JavaArray<int>` in
order to support invoking the C# interface:

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

If `int[]` would be preferred over `JavaList<int>`, then
[JNIEnv.GetArray()](xref:Android.Runtime.JNIEnv.GetArray*)
could be used instead:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Note, however, that `JNIEnv.GetArray` copies the entire array between
VMs, so for large arrays this could result in lots of added GC
pressure.

### Complete Invoker Definition

The [complete IAdderProgressInvoker definition](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## JNI Object References

Many JNIEnv methods return *JNI* *object references*, which are similar
to `GCHandle`s. JNI provides three different types of object
references: local references, global references, and weak global
references. All three are represented as `System.IntPtr`, *but* (as per
the JNI Function Types section) not all `IntPtr`s returned from
`JNIEnv` methods are references. For example,
[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)
returns an `IntPtr`, but it doesn't return an object reference, it
returns a `jmethodID`. Consult the
[JNI function documentation](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
for details.

Local references are created by *most* reference-creating methods.
Android only allows a limited number of local references to exist at
any given time, usually 512. Local references can be deleted via
[JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*).
Unlike JNI, not all reference JNIEnv methods which return object
references return local references;
[JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*)
returns a *global* reference. It is strongly recommended that you
delete local references as quickly as you can, possibly by constructing
a
[Java.Lang.Object](xref:Java.Lang.Object)
around the object and specifying `JniHandleOwnership.TransferLocalRef`
to the
[Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*)
constructor.

Global references are created by
[JNIEnv.NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*)
and
[JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*).
They can be destroyed with
[JNIEnv.DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*).
Emulators have a limit of 2,000 outstanding global references, while
hardware devices have a limit of around 52,000 global references.

Weak global references are only available on Android v2.2 (Froyo) and
later. Weak global references can be deleted with
[JNIEnv.DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*).

### Dealing With JNI Local References

The
[JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*),
[JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*),
[JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*),
[JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*)
and
[JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)
methods return an `IntPtr` which contains a JNI local reference to a
Java object, or `IntPtr.Zero` if Java returned `null`. Due to the
limited number of local references that can be outstanding at once (512
entries), it is desirable to ensure that the references are deleted in
a timely fashion. There are three ways that local references can be
dealt with: explicitly deleting them, creating a `Java.Lang.Object`
instance to hold them, and using `Java.Lang.Object.GetObject<T>()` to
create a managed callable wrapper around them.

### Explicitly Deleting Local References

[JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)
is used to delete local references. Once the local reference has been
deleted, it cannot be used anymore, so care must be taken to ensure
that `JNIEnv.DeleteLocalRef` is the last thing done with the local
reference.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### Wrapping with Java.Lang.Object

`Java.Lang.Object` provides a
[Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*)
constructor which can be used to wrap an exiting JNI reference. The
[JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership)
parameter determines how the `IntPtr` parameter should be treated:

- [JniHandleOwnership.DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer)
    &ndash; The created `Java.Lang.Object` instance will create a new global
    reference from the `handle` parameter, and `handle` is unchanged.
    The caller is responsible to freeing `handle` , if necessary.

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef)
    &ndash; The created `Java.Lang.Object` instance will create a new global
    reference from the `handle` parameter, and `handle` is deleted with
    [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)
    . The caller must not free `handle` , and must not use `handle`
    after the constructor finishes executing.

- [JniHandleOwnership.TransferGlobalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef)
    &ndash; The created `Java.Lang.Object` instance will take over ownership
    of the `handle` parameter. The caller must not free `handle` .

Since the JNI method invocation methods return local refs,
`JniHandleOwnership.TransferLocalRef` would normally be used:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

The created global reference will not be freed until the
`Java.Lang.Object` instance is garbage collected. If you are able to,
disposing of the instance will free up the global reference, speeding
up garbage collections:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### Using Java.Lang.Object.GetObject&lt;T&gt;()

`Java.Lang.Object` provides a
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object.GetObject*)
method that can be used to create a managed callable wrapper of the
specified type.

The type `T` must fulfill the following requirements:

1. `T` must be a reference type.

1. `T` must implement the `IJavaObject` interface.

1. If `T` is not an abstract class or interface, then `T` must provide
    a constructor with the parameter types `(IntPtr,
    JniHandleOwnership)` .

1. If `T` is an abstract class or an interface, there *must* be an
    *invoker* available for `T` . An invoker is a non-abstract type
    that inherits `T` or implements `T` , and has the same name as `T`
    with an Invoker suffix. For example, if T is the interface
    `Java.Lang.IRunnable` , then the type `Java.Lang.IRunnableInvoker`
    must exist and must contain the required `(IntPtr,
    JniHandleOwnership)` constructor.

Since the JNI method invocation methods return local refs,
`JniHandleOwnership.TransferLocalRef` would normally be used:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types"></a>

## Looking up Java Types

To lookup a field or method in JNI, the declaring type for the
field or method must be looked up first. The
[Android.Runtime.JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*))
method is used to lookup Java types. The string parameter is the
*simplified type reference* or the *full type reference* for the Java
type. See the [JNI Type References section](#_JNI_Type_References) for
details about simplified and full type references.

Note: Unlike every other `JNIEnv` method which returns object
instances, `FindClass` returns a global reference, not a local
reference.

<a name="_Instance_Fields"></a>

## Instance Fields

Fields are manipulated through *field IDs*. Field IDs are obtained via
[JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*),
which requires the class that the field is defined in, the name of the
field, and the [JNI Type Signature](#JNI_Type_Signatures) of the field.

Field IDs do not need to be freed, and are valid as long as the
corresponding Java type is loaded. (Android does not currently support
class unloading.)

There are two sets of methods for manipulating instance fields: one for
reading instance fields and one for writing instance fields. All sets
of methods require a field ID to read or write the field value.

### Reading Instance Field Values

The set of methods for reading instance field values follows the naming
pattern:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

where `*` is the type of the field:

- [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*)
    &ndash; Read the value of any instance field that isn't a builtin type,
    such as `java.lang.Object` , arrays, and interface types. The value
    returned is a JNI local reference.

- [JNIEnv.GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*)
    &ndash; Read the value of `bool` instance fields.

- [JNIEnv.GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*)
    &ndash; Read the value of `sbyte` instance fields.

- [JNIEnv.GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*)
    &ndash; Read the value of `char` instance fields.

- [JNIEnv.GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*)
    &ndash; Read the value of `short` instance fields.

- [JNIEnv.GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*)
    &ndash; Read the value of `int` instance fields.

- [JNIEnv.GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*)
    &ndash; Read the value of `long` instance fields.

- [JNIEnv.GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*)
    &ndash; Read the value of `float` instance fields.

- [JNIEnv.GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*)
    &ndash; Read the value of `double` instance fields.

### Writing Instance Field Values

The set of methods for writing instance field values follows the naming
pattern:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

where *Type* is the type of the field:

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of any field that isn't a builtin type, such as
    `java.lang.Object` , arrays, and interface types. The `IntPtr`
    value may be a JNI local reference, JNI global reference, JNI weak
    global reference, or `IntPtr.Zero` (for `null` ).

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `bool` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `sbyte` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `char` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `short` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `int` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `long` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `float` instance fields.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))
    &ndash; Write the value of `double` instance fields.

<a name="_Static_Fields"></a>

## Static Fields

Static Fields are manipulated through *field IDs*. Field IDs are
obtained via
[JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticFieldID*),
which requires the class that the field is defined in, the name of the
field, and the [JNI Type Signature](#JNI_Type_Signatures) of the field.

Field IDs do not need to be freed, and are valid as long as the
corresponding Java type is loaded. (Android does not currently support
class unloading.)

There are two sets of methods for manipulating static fields: one for
reading instance fields and one for writing instance fields. All sets
of methods require a field ID to read or write the field value.

### Reading Static Field Values

The set of methods for reading static field values follows the naming
pattern:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

where `*` is the type of the field:

- [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*)
    &ndash; Read the value of any static field that isn't a builtin type,
    such as `java.lang.Object` , arrays, and interface types. The value
    returned is a JNI local reference.

- [JNIEnv.GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*)
    &ndash; Read the value of `bool` static fields.

- [JNIEnv.GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*)
    &ndash; Read the value of `sbyte` static fields.

- [JNIEnv.GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*)
    &ndash; Read the value of `char` static fields.

- [JNIEnv.GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*)
    &ndash; Read the value of `short` static fields.

- [JNIEnv.GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*)
    &ndash; Read the value of `long` static fields.

- [JNIEnv.GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*)
    &ndash; Read the value of `float` static fields.

- [JNIEnv.GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*)
    &ndash; Read the value of `double` static fields.

### Writing Static Field Values

The set of methods for writing static field values follows the naming
pattern:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

where *Type* is the type of the field:

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of any static field that isn't a builtin type,
    such as `java.lang.Object` , arrays, and interface types. The
    `IntPtr` value may be a JNI local reference, JNI global reference,
    JNI weak global reference, or `IntPtr.Zero` (for `null` ).

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `bool` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `sbyte` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `char` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `short` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `int` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `long` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `float` static fields.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))
    &ndash; Write the value of `double` static fields.

<a name="_Instance_Methods"></a>

## Instance Methods

Instance Methods are invoked through *method IDs*. Method IDs are
obtained via
[JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*),
which requires the type that the method is defined in, the name of the
method, and the [JNI Type Signature](#JNI_Type_Signatures) of the method.

Method IDs do not need to be freed, and are valid as long as the
corresponding Java type is loaded. (Android does not currently support
class unloading.)

There are two sets of methods for invoking methods: one for invoking
methods virtually, and one for invoking methods non-virtually. Both
sets of methods require a method ID to invoke the method, and
non-virtual invocation also requires that you specify which class
implementation should be invoked.

Interface methods can only be looked up within the declaring type;
methods that come from extended/inherited interfaces cannot be looked
up. See the later Binding Interfaces / Invoker Implementation section
for more details.

Any method declared in the class or any base class or implemented interface
can be looked up.

### Virtual Method Invocation

The set of methods for invoking methods virtually follows the naming
pattern:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

where `*` is the return type of the method.

- [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*)
    &ndash; Invoke a method which returns a non-builtin type, such as
    `java.lang.Object` , arrays, and interfaces. The value returned is
    a JNI local reference.

- [JNIEnv.CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*)
    &ndash; Invoke a method which returns a `bool` value.

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*)
    &ndash; Invoke a method which returns a `sbyte` value.

- [JNIEnv.CallCharMethod](xref:Android.Runtime.JNIEnv.CallCharMethod*)
    &ndash; Invoke a method which returns a `char` value.

- [JNIEnv.CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*)
    &ndash; Invoke a method which returns a `short` value.

- [JNIEnv.CallLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*)
    &ndash; Invoke a method which returns a `long` value.

- [JNIEnv.CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*)
    &ndash; Invoke a method which returns a `float` value.

- [JNIEnv.CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*)
    &ndash; Invoke a method which returns a `double` value.

### Non-virtual Method Invocation

The set of methods for invoking methods non-virtually follows the naming
pattern:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

where `*` is the return type of the method. Non-virtual method
invocation is usually used to invoke the base method of a virtual method.

- [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*)
    &ndash; Non-virtually invoke a method which returns a non-builtin type,
    such as `java.lang.Object` , arrays, and interfaces. The value
    returned is a JNI local reference.

- [JNIEnv.CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*)
    &ndash; Non-virtually invoke a method which returns a `bool` value.

- [JNIEnv.CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*)
    &ndash; Non-virtually invoke a method which returns a `sbyte` value.

- [JNIEnv.CallNonvirtualCharMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*)
    &ndash; Non-virtually invoke a method which returns a `char` value.

- [JNIEnv.CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*)
    &ndash; Non-virtually invoke a method which returns a `short` value.

- [JNIEnv.CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*)
    &ndash; Non-virtually invoke a method which returns a `long` value.

- [JNIEnv.CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*)
    &ndash; Non-virtually invoke a method which returns a `float` value.

- [JNIEnv.CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*)
    &ndash; Non-virtually invoke a method which returns a `double` value.

<a name="_Static_Methods"></a>

## Static Methods

Static Methods are invoked through *method IDs*. Method IDs are
obtained via
[JNIEnv.GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*),
which requires the type that the method is defined in, the name of the
method, and the [JNI Type Signature](#JNI_Type_Signatures) of the method.

Method IDs do not need to be freed, and are valid as long as the
corresponding Java type is loaded. (Android does not currently support class
unloading.)

### Static Method Invocation

The set of methods for invoking methods virtually follows the naming
pattern:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

where `*` is the return type of the method.

- [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)
    &ndash; Invoke a static method which returns a non-builtin type, such as
    `java.lang.Object` , arrays, and interfaces. The value returned is
    a JNI local reference.

- [JNIEnv.CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*)
    &ndash; Invoke a static method which returns a `bool` value.

- [JNIEnv.CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*)
    &ndash; Invoke a static method which returns a `sbyte` value.

- [JNIEnv.CallStaticCharMethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*)
    &ndash; Invoke a static method which returns a `char` value.

- [JNIEnv.CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*)
    &ndash; Invoke a static method which returns a `short` value.

- [JNIEnv.CallStaticLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*)
    &ndash; Invoke a static method which returns a `long` value.

- [JNIEnv.CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*)
    &ndash; Invoke a static method which returns a `float` value.

- [JNIEnv.CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*)
    &ndash; Invoke a static method which returns a `double` value.

<a name="JNI_Type_Signatures"></a>

## JNI Type Signatures

[JNI Type Signatures](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432)
are [JNI Type References](#_JNI_Type_References) (though not simplified
type references), except for methods. With methods, the JNI Type
Signature is an open parenthesis `'('`, followed by the type references
for all of the parameter types concatenated together (with no
separating commas or anything else), followed by a closing parenthesis
`')'`, followed by the JNI type reference of the method return type.

For example, given the Java method:

```java
long f(int n, String s, int[] array);
```

The JNI type signature would be:

```csharp
(ILjava/lang/String;[I)J
```

In general, it is *strongly* recommended to use the `javap` command to
determine JNI signatures. For example, the JNI Type Signature of the
[java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String))
method is "(Ljava/lang/String;)Ljava/lang/Thread$State;", while the JNI
Type Signature of the
[java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values)
method is "()[Ljava/lang/Thread$State;". Watch out for the trailing
semicolons; those *are* part of the JNI type signature.

<a name="_JNI_Type_References"></a>

## JNI Type References

JNI type references are different from Java type references. You cannot use
fully qualified Java type names such as `java.lang.String` with JNI,
you must instead use the JNI variations `"java/lang/String"` or `"Ljava/lang/String;"`, depending on context; see below for details.
There are four types of JNI type references:

- **built-in**
- **simplified**
- **type**
- **array**

### Built-in Type References

Built-in type references are a single character, used to reference built-in
value types. The mapping is as follows:

- `"B"` for  `sbyte` .
- `"S"` for  `short` .
- `"I"` for  `int` .
- `"J"` for  `long` .
- `"F"` for  `float` .
- `"D"` for  `double` .
- `"C"` for  `char` .
- `"Z"` for  `bool` .
- `"V"` for  `void` method return types.

<a name="_Simplified_Type_References_1"></a>

### Simplified Type References

Simplified type references can only be used in
[JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*)).
There are two ways to derive a simplified type reference:

1. From a fully-qualified Java name, replace every `'.'` within
    the package name and before the type name with `'/'` , and
    every `'.'` within a type name with `'$'` .

1. Read the output of `'unzip -l android.jar | grep JavaName'` .

Either of the two will result in the Java type
[java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html)
being mapped to the simplified type reference `java/lang/Thread$State`.

### Type References

A type reference is a built-in type reference or a simplified type
reference with an `'L'` prefix and a `';'` suffix. For the Java type
[java.lang.String](https://developer.android.com/reference/java/lang/String.html),
the simplified type reference is `"java/lang/String"`, while the type
reference is `"Ljava/lang/String;"`.

Type references are used with Array type references and with JNI
Signatures.

An additional way to obtain a type reference is by reading the output
of `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
Depending on the type involved, you can use a constructor declaration
or method return type to determine the JNI name. For example:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` is a Java enum type, so we can use the Signature of the
`valueOf` method to determine that the type reference is
Ljava/lang/Thread$State;.

### Array Type References

Array type references are `'['` prefixed to a JNI type reference.
Simplified type references cannot be used when specifying arrays.

For example, `int[]` is `"[I"`, `int[][]` is `"[[I"`, and `java.lang.Object[]` is `"[Ljava/lang/Object;"`.

## Java Generics and Type Erasure

*Most* of the time, as seen through JNI, Java generics *do not exist*.
There are some "wrinkles," but those wrinkles are in how Java interacts
with generics, not with how JNI looks up and invokes generic members.

There is no difference between a generic type or member and a
non-generic type or member when interacting through JNI. For example,
the generic type
[java.lang.Class&lt;T&gt;](https://developer.android.com/reference/java/lang/Class.html)
is also the "raw" generic type `java.lang.Class`, both of which have
the same simplified type reference, `"java/lang/Class"`.

## Java Native Interface Support

[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv)
is managed wrapper for the Jave Native Interface (JNI). JNI Functions
are declared within the
[Java Native Interface Specification](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html),
though the methods have been changed to remove the explicit `JNIEnv*`
parameter and `IntPtr` is used instead of `jobject`, `jclass`,
`jmethodID`, etc. For example, consider the
[JNI NewObject function](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

This is exposed as the [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) method:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Translating between the two calls is reasonably straightforward. In C you would have:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

The C# equivalent would be:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Once you have a Java Object instance held in an IntPtr, you'll probably
want to do something with it. You can use JNIEnv methods such as
[JNIEnv.CallVoidMethod()](xref:Android.Runtime.JNIEnv.CallVoidMethod*)
to do so, but if there is already an analogue C# wrapper then you'll
want to construct a wrapper over the JNI reference. You can do so
through the
[Extensions.JavaCast\<T>](xref:Android.Runtime.Extensions.JavaCast*)
extension method:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

You can also use the
[Java.Lang.Object.GetObject\<T>](xref:Java.Lang.Object.GetObject*)
method:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Furthermore, all of the JNI functions have been modified by removing
the `JNIEnv*` parameter present in every JNI function.

## Summary

Dealing directly with JNI is a terrible experience that should be
avoided at all costs. Unfortunately, it's not always avoidable;
hopefully this guide will provide some assistance when you hit the
unbound Java cases with Mono for Android.

## Related links

- [Java Native Interface Specification](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface Functions](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
