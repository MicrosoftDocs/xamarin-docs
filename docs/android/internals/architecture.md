---
title: "Architecture"
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
---

# Architecture

Xamarin.Android applications run within the Mono execution environment.
This execution environment runs side-by-side with the Android Runtime
(ART) virtual machine. Both runtime environments run on top of the
Linux kernel and expose various APIs to the user code that allows
developers to access the underlying system. The Mono runtime is written
in the C language.

You can be using the
[System](xref:System),
[System.IO](xref:System.IO),
[System.Net](xref:System.Net)
and the rest of the .NET class libraries to access the underlying Linux
operating system facilities.

On Android, most of the system facilities like Audio, Graphics, OpenGL
and Telephony are not available directly to native applications, they
are only exposed through the Android Runtime Java APIs residing
in one of the
[Java](xref:Java.Lang).* namespaces
or the [Android](xref:Android).*
namespaces. The architecture is roughly like this:

[![Diagram of Mono and ART above the kernel and below .NET/Java + bindings](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android developers access the various features in the operating
system either by calling into .NET APIs that they know (for low-level
access) or using the classes exposed in the Android namespaces which
provides a bridge to the Java APIs that are exposed by the Android
Runtime.

For more information on how the Android classes communicate with the
Android Runtime classes see the
[API Design](~/android/internals/api-design.md) document.

## Application Packages

Android application packages are ZIP containers with a *.apk* file
extension. Xamarin.Android application packages have the same structure
and layout as normal Android packages, with the following additions:

- The application assemblies (containing IL) are *stored*
    uncompressed within the *assemblies* folder. During process startup
    in Release builds the *.apk* is *mmap()* ed into the process and
    the assemblies are loaded from memory. This permits faster app
    startup, as assemblies do not need to be extracted prior to
    execution.  
- *Note:* Assembly location information such as
    [Assembly.Location](xref:System.Reflection.Assembly.Location)
    and [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *cannot be relied upon* in Release builds. They do not exist as
    distinct filesystem entries, and they have no usable location.

- Native libraries containing the Mono runtime are present within the
    *.apk* . A Xamarin.Android application must contain native
    libraries for the desired/targeted Android architectures, e.g.
    *armeabi* , *armeabi-v7a* , *x86* . Xamarin.Android applications
    cannot run on a platform unless it contains the appropriate runtime
    libraries.

Xamarin.Android applications also contain *Android Callable Wrappers*
to allow Android to call into managed code.

## Android Callable Wrappers

- **Android callable wrappers** are a
  [JNI](https://en.wikipedia.org/wiki/Java_Native_Interface) bridge
  which are used any time the Android runtime needs to invoke managed
  code. Android callable wrappers are how virtual methods can be
  overridden and Java interfaces can be implemented. See the
  [Java Integration Overview](~/android/platform/java-integration/index.md)
  doc for more.

<a name="Managed_Callable_Wrappers"></a>

## Managed Callable Wrappers

Managed callable wrappers are a JNI bridge which are used any time
managed code needs to invoke Android code and provide support for
overriding virtual methods and implementing Java interfaces. The entire
[Android](xref:Android).* and
related namespaces are managed callable wrappers generated via
[.jar binding](~/android/platform/binding-java-library/index.md).
Managed callable wrappers are responsible for converting between
managed and Android types and invoking the underlying Android platform
methods via JNI.

Each created managed callable wrapper holds a Java global reference,
which is accessible through the
[Android.Runtime.IJavaObject.Handle](xref:Android.Runtime.IJavaObject.Handle)
property. Global references are used to provide the mapping between
Java instances and managed instances. Global references are a limited
resource: emulators allow only 2000 global references to exist at a
time, while most hardware allows over 52,000 global references to exist
at a time.

To track when global references are created and destroyed, you 
can set the
[debug.mono.log](~/android/troubleshooting/index.md)
system property to contain
[gref](~/android/troubleshooting/index.md).

Global references can be explicitly freed by calling
[Java.Lang.Object.Dispose()](xref:Java.Lang.Object.Dispose)
on the managed callable wrapper. This will remove the mapping between
the Java instance and the managed instance and allow the Java instance
to be collected. If the Java instance is re-accessed from managed code,
a new managed callable wrapper will be created for it.

Care must be exercised when disposing of Managed Callable Wrappers if
the instance can be inadvertently shared between threads, as disposing
the instance will impact references from any other threads. For maximum
safety, only `Dispose()` of instances which have been allocated via
`new` *or* from methods which you *know* always allocate new instances
and not cached instances which may cause accidental instance sharing
between threads.

## Managed Callable Wrapper Subclasses

Managed callable wrapper subclasses are where all the "interesting"
application-specific logic may live. These include custom
[Android.App.Activity](xref:Android.App.Activity)
subclasses (such as the
[Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13)
type in the default project template). (Specifically, these are any
*Java.Lang.Object* subclasses which do *not* contain a
[RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
custom attribute or
[RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw)
is *false*, which is the default.)

Like managed callable wrappers, managed callable wrapper subclasses
also contain a global reference, accessible through the
[Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle)
property. Just as with managed callable wrappers, global references can
be explicitly freed by calling
[Java.Lang.Object.Dispose()](xref:Java.Lang.Object.Dispose).
Unlike managed callable wrappers, *great care* should be taken before
disposing of such instances, as *Dispose()*-ing of the instance will
break the mapping between the Java instance (an instance of an Android
Callable Wrapper) and the managed instance.

### Java Activation

When an
[Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md)
(ACW) is created from Java, the ACW constructor will cause the
corresponding C# constructor to be invoked. For example, the ACW for
*MainActivity* will contain a default constructor which will invoke
*MainActivity*'s default constructor. (This is done through the
*TypeManager.Activate()* call within the ACW constructors.)

There is one other constructor signature of consequence: the *(IntPtr,
JniHandleOwnership)* constructor. The *(IntPtr, JniHandleOwnership)*
constructor is invoked whenever a Java object is exposed to managed
code and a Managed Callable Wrapper needs to be constructed to manage
the JNI handle. This is usually done automatically.

There are two scenarios in which the *(IntPtr, JniHandleOwnership)*
constructor must be manually provided on a Managed Callable Wrapper
subclass:

1. [Android.App.Application](xref:Android.App.Application)
   is subclassed. *Application* is special; the default *Applicaton*
   constructor will *never* be invoked, and the
   [(IntPtr, JniHandleOwnership) constructor must instead be provided](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Virtual method invocation from a base class constructor.

Note that (2) is a leaky abstraction. In Java, as in C#, calls to
virtual methods from a constructor always invoke the most derived
method implementation. For example, the
[TextView(Context, AttributeSet, int) constructor](xref:Android.Widget.TextView#ctor*)
invokes the virtual method
[TextView.getDefaultMovementMethod()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()),
which is bound as the
[TextView.DefaultMovementMethod property](xref:Android.Widget.TextView.DefaultMovementMethod).
Thus, if a type
[LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs)
were to (1)
[subclass TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26),
(2)
[override TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45),
and (3)
[activate an instance of that class via XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29)
the overridden *DefaultMovementMethod* property would be invoked before
the ACW constructor had a chance to execute, and it would occur before
the C# constructor had a chance to execute.

This is supported by instantiating an instance LogTextBox through the
[LogTextView(IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28)
constructor when the ACW LogTextBox instance first enters managed code,
and then invoking the
[LogTextBox(Context, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41)
constructor *on the same instance* when the ACW constructor executes.

Order of events:

1. Layout XML is loaded into a
    [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2. Android instantiates the Layout object graph, and instantiates an
    instance of *monodroid.apidemo.LogTextBox* , the ACW for
    *LogTextBox* .

3. The *monodroid.apidemo.LogTextBox* constructor executes the
    [android.widget.TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29)
    constructor.

4. The *TextView* constructor invokes
    *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5. *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes
    *LogTextBox.n_getDefaultMovementMethod()* , which invokes
    *TextView.n_GetDefaultMovementMethod()* , which invokes
    [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](xref:Java.Lang.Object.GetObject*)
    .

6. *Java.Lang.Object.GetObject&lt;TextView&gt;()* checks to see if
    there is already a corresponding C# instance for *handle* . If
    there is, it is returned. In this scenario, there isn't, so
    *Object.GetObject&lt;T&gt;()* must create one.

7. *Object.GetObject&lt;T&gt;()* looks for the *LogTextBox(IntPtr,
    JniHandleOwneship)* constructor, invokes it, creates a mapping
    between *handle* and the created instance, and returns the created
    instance.

8. *TextView.n_GetDefaultMovementMethod()* invokes the
    *LogTextBox.DefaultMovementMethod* property getter.

9. Control returns to the *android.widget.TextView* constructor, which
    finishes execution.

10. The *monodroid.apidemo.LogTextBox* constructor executes, invoking
    *TypeManager.Activate()* .

11. The *LogTextBox(Context, IAttributeSet, int)* constructor executes
    *on the same instance created in (7)* .

12. If the (IntPtr, JniHandleOwnership) constructor cannot be found, then a
    System.MissingMethodException](xref:System.MissingMethodException)
    will be thrown.

<a name="Premature_Dispose_Calls"></a>

### Premature Dispose() Calls

There is a mapping between a JNI handle and the corresponding C#
instance. Java.Lang.Object.Dispose() breaks this mapping. If a JNI
handle enters managed code after the mapping has been broken, it looks
like Java Activation, and the *(IntPtr, JniHandleOwnership)*
constructor will be checked for and invoked. If the constructor doesn't
exist, then an exception will be thrown.

For example, given the following Managed Callable Wraper subclass:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

If we create an instance, Dispose() of it, and cause the Managed
Callable Wrapper to be re-created:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

The program will die:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

If the subclass does contain an *(IntPtr, JniHandleOwnership)*
constructor, then a *new* instance of the type will be created. As a
result, the instance will appear to "lose" all instance data, as it's a
new instance. (Note that the Value is null.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Only *Dispose()* of managed callable wrapper subclasses when you know
that the Java object will not be used anymore, or the subclass contains
no instance data and a *(IntPtr, JniHandleOwnership)* constructor has
been provided.

## Application Startup

When an activity, service, etc. is launched, Android will first check
to see if there is already a process running to host the
activity/service/etc. If no such process exists, then a new process
will be created, the
[AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro.html)
is read, and the type specified in the
[/manifest/application/@android:name](https://developer.android.com/guide/topics/manifest/application-element.html#nm)
attribute is loaded and instantiated. Next, all types specified by the
[/manifest/application/provider/@android:name](https://developer.android.com/guide/topics/manifest/provider-element.html#nm)
attribute values are instantiated and have their
[ContentProvider.attachInfo%28)](xref:Android.Content.ContentProvider.AttachInfo*)
method invoked. Xamarin.Android hooks into this by adding a
*mono.MonoRuntimeProvider* *ContentProvider* to AndroidManifest.xml
during the build process. The *mono.MonoRuntimeProvider.attachInfo()*
method is responsible for loading the Mono runtime into the process.
Any attempts to use Mono prior to this point will fail. ( *Note*: This
is why types which subclass
[Android.App.Application](xref:Android.App.Application)
need to provide an
[(IntPtr, JniHandleOwnership) constructor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103),
as the Application instance is created before Mono can be initialized.)

Once process initialization has completed, `AndroidManifest.xml` is
consulted to find the class name of the activity/service/etc. to
launch. For example, the
[/manifest/application/activity/@android:name attribute](https://developer.android.com/guide/topics/manifest/activity-element.html#nm)
is used to determine the name of an Activity to load. For Activities,
this type must inherit
[android.app.Activity](xref:Android.App.Activity).
The specified type is loaded via
[Class.forName()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String))
(which requires that the type be a Java type, hence the Android
Callable Wrappers), then instantiated. Creation of an Android Callable
Wrapper instance will trigger creation of an instance of the
corresponding C# type. Android will then invoke
[Activity.onCreate(Bundle)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle))
, which will cause the corresponding
[Activity.OnCreate(Bundle)](xref:Android.App.Activity.OnCreate*)
to be invoked, and you're off to the races.
