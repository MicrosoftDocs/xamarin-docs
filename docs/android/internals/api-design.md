---
title: "Xamarin.Android API Design Principles"
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---
# Xamarin.Android API Design Principles

In addition to the core Base Class Libraries that are part of Mono,
Xamarin.Android ships with bindings for various Android APIs to allow
developers to create native Android applications with Mono.

At the core of Xamarin.Android there is an interop engine that bridges
the C# world with the Java world and provides developers with access to
the Java APIs from C# or other .NET languages.

## Design Principles

These are some of our design principles for the Xamarin.Android binding

- Conform to the  [.NET Framework Design Guidelines](/dotnet/standard/design-guidelines/).

- Allow developers to subclass Java classes.

- Subclass should work with C# standard constructs.

- Derive from an existing class.

- Call base constructor to chain.

- Overriding methods should be done with C#'s override system.

- Make common Java tasks easy, and hard Java tasks possible.

- Expose JavaBean properties as C# properties.

- Expose a strongly typed API:

  - Increase type-safety.

  - Minimize runtime errors.

  - Get IDE intellisense on return types.

  - Allows for IDE popup documentation.

- Encourage in-IDE exploration of the APIs:

  - Utilize Framework Alternatives to Minimize Java Classlib exposure.

  - Expose C# delegates (lambdas, anonymous methods and System.Delegate)
    instead of single-method interfaces when appropriate and applicable.

  - Provide a mechanism to call arbitrary Java libraries (
    [Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv)).

## Assemblies

Xamarin.Android includes a number of assemblies that constitute the
*MonoMobile Profile*. The
[Assemblies](~/cross-platform/internals/available-assemblies.md) page has more
information.

The bindings to the Android platform are contained in the
`Mono.Android.dll` assembly. This assembly contains the entire binding
for consuming Android APIs and communicating with the Android runtime
VM.

## Binding Design

### Collections

The Android APIs utilize the java.util collections extensively to
provide lists, sets, and maps. We expose these elements using the
[System.Collections.Generic](xref:System.Collections.Generic)
interfaces in our binding. The fundamental mappings are:

- [java.util.Set\<E>](https://developer.android.com/reference/java/util/Set.html) maps to
  system type [ICollection\<T>](xref:System.Collections.Generic.ICollection`1),
  helper class [Android.Runtime.JavaSet\<T>](xref:Android.Runtime.JavaSet`1).

- [java.util.List\<E>](https://developer.android.com/reference/java/util/List.html) maps to
  system type [IList\<T>](xref:System.Collections.Generic.IList`1),
  helper class [Android.Runtime.JavaList\<T>](xref:Android.Runtime.JavaList`1).

- [java.util.Map<K,V>](https://developer.android.com/reference/java/util/Map.html) maps to
  system type [IDictionary<TKey,TValue>](xref:System.Collections.Generic.IDictionary`2),
  helper class [Android.Runtime.JavaDictionary<K,V>](xref:Android.Runtime.JavaDictionary`2).

- [java.util.Collection\<E>](https://developer.android.com/reference/java/util/Collection.html) maps to
  system type [ICollection\<T>](xref:System.Collections.Generic.ICollection`1),
  helper class [Android.Runtime.JavaCollection\<T>](xref:Android.Runtime.JavaCollection`1).

We have provided helper classes to facilitate faster copyless
marshaling of these types. When possible, we recommend using these
provided collections instead of the framework provided implementation,
like
[`List<T>`](xref:System.Collections.Generic.List`1) or
[`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2). The
[Android.Runtime](xref:Android.Runtime) implementations
utilize a native Java collection internally and therefore do not
require copying to and from a native collection when passing to an
Android API member.

You can pass any interface implementation to an Android method
accepting that interface, e.g. pass a `List<int>` to the
[ArrayAdapter&lt;int&gt;(Context, int, IList&lt;int&gt;)](xref:Android.Widget.ArrayAdapter`1)
constructor. *However*, for all implementations *except* for the
Android.Runtime implementations, this involves *copying* the list from
the Mono VM into the Android runtime VM. If the list is later changed
within the Android runtime (e.g. by invoking the
[ArrayAdapter&lt;T&gt;.Add(T)](xref:Android.Widget.ArrayAdapter`1.Add*)
method), those changes *will not* be visible in managed code. If a
`JavaList<int>` were used, those changes would be visible.

Rephrased, collections interface implementations that are *not* one of
the above listed **Helper Class**es only marshal [In]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```

### Properties

Java methods are transformed into properties, when appropriate:

- The Java method pair `T getFoo()` and `void setFoo(T)` are
  transformed into the `Foo` property. Example:
  [Activity.Intent](xref:Android.App.Activity.Intent).

- The Java method `getFoo()` is transformed into the read-only Foo
  property. Example:
  [Context.PackageName](xref:Android.Content.Context.PackageName).

- Set-only properties are not generated.

- Properties are *not* generated if the property type would be an
  array.

### Events and Listeners

The Android APIs are built on top of Java and its components follow the
Java pattern for hooking up event listeners. This pattern tends to be
cumbersome as it requires the user to create an anonymous class and
declare the methods to override, for example, this is how things would
be done in Android with Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

The equivalent code in C# using events would be:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Note that both of the above mechanisms are available with
Xamarin.Android. You can implement a listener interface and attach it
with View.SetOnClickListener, or you can attach a delegate created via
any of the usual C# paradigms to the Click event.

When the listener callback method has a void return, we create API
elements based on an
[EventHandler&lt;TEventArgs&gt;](xref:System.EventHandler`1)
delegate. We generate an event like the above example for these
listener types. However, if the listener callback returns a non-void
and non- **boolean** value, events and EventHandlers are not used. We
instead generate a specific delegate for the signature of the callback
and add properties instead of events. The reason is to deal with
delegate invocation order and return handling. This approach mirrors
what is done with the Xamarin.iOS API.

C# events or properties are only automatically generated if the Android
event-registration method:

1. Has a `set` prefix, e.g.
   [*set*OnClickListener](xref:Android.Views.View.SetOnClickListener*).

1. Has a `void` return type.

1. Accepts only one parameter, the parameter type is an interface, the
   interface has only one method, and the interface name ends in
   `Listener` , e.g.
   [View.OnClick *Listener*](xref:Android.Views.View.IOnClickListener).

Furthermore, if the Listener interface method has a return type of
**boolean** instead of **void**, then the generated *EventArgs*
subclass will contain a *Handled* property. The value of the *Handled*
property is used as the return value for the *Listener* method, and it
defaults to `true`.

For example, the Android
[View.setOnKeyListener()](xref:Android.Views.View.SetOnKeyListener*)
method accepts the
[View.OnKeyListener](xref:Android.Views.View.IOnKeyListener)
interface, and the
[View.OnKeyListener.onKey(View, int, KeyEvent)](xref:Android.Views.View.IOnKeyListener.OnKey*)
method has a boolean return type. Xamarin.Android generates a
corresponding
[View.KeyPress](xref:Android.Views.View.KeyPress) event, which
is an
[EventHandler&lt;View.KeyEventArgs&gt;](xref:Android.Views.View.KeyEventArgs).
The *KeyEventArgs* class in turn has a
[View.KeyEventArgs.Handled](xref:Android.Views.View.KeyEventArgs.Handled)
property, which is used as the return value for the
*View.OnKeyListener.onKey()* method.

We intend to add overloads for other methods and ctors to expose the
delegate-based connection. Also, listeners with multiple callbacks
require some additional inspection to determine if implementing
individual callbacks is reasonable, so we are converting these as they
are identified. If there is no corresponding event, listeners must be
used in C#, but please bring any that you think could have delegate
usage to our attention. We have also done some conversions of
interfaces without the "Listener" suffix when it was clear they would
benefit from a delegate alternative.

All of the listeners interfaces implement the
[`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
interface, because of the implementation details of the binding, so
listener classes must implement this interface. This can be done by
implementing the listener interface on a subclass of
[Java.Lang.Object](xref:Java.Lang.Object) or any other wrapped
Java object, such as an Android activity.

### Runnables

Java utilizes the
[java.lang.Runnable](xref:Java.Lang.Runnable) interface to
provide a delegation mechanism. The
[java.lang.Thread](xref:Java.Lang.Thread) class is a notable
consumer of this interface. Android has employed the interface in the
API as well.
[Activity.runOnUiThread()](xref:Android.App.Activity.RunOnUiThread*)
and
[View.post()](xref:Android.Views.View.Post*)
are notable examples.

The `Runnable` interface contains a single void method,
[run()](xref:Java.Lang.Runnable.Run). It therefore lends
itself to binding in C# as a
[System.Action](xref:System.Action)
delegate. We have provided overloads in the binding which accept an
`Action` parameter for all API members which consume a `Runnable` in
the native API, e.g.
[Activity.RunOnUiThread()](xref:Android.App.Activity.RunOnUiThread*)
and
[View.Post()](xref:Android.Views.View.Post*).

We left the
[IRunnable](xref:Java.Lang.IRunnable) overloads in place instead
of replacing them since several types implement the interface and can
therefore be passed as runnables directly.

### Inner Classes

Java has two different types of
[nested classes](https://download.oracle.com/javase/tutorial/java/javaOO/nested.html):
static nested classes and non-static classes.

Java static nested classes are identical to C# nested types.

Non-static nested classes, also called *inner classes*, are
significantly different. They contain an implicit reference to an
instance of their enclosing type and cannot contain static members
(among other differences outside the scope of this overview).

When it comes to binding and C# use, static nested classes are treated
as normal nested types. Inner classes, meanwhile, have two significant
differences:

1. The implicit reference to the containing type must be provided
   explicitly as a constructor parameter.

1. When inheriting from an inner class, the inner class *must* be
   nested within a type that inherits from the containing type of the
   base inner class, and the derived type must provide a constructor of
   the same type as the C# containing type.

For example, consider the
[Android.Service.Wallpaper.WallpaperService.Engine](xref:Android.Service.Wallpaper.WallpaperService.Engine)
inner class. Since it's an inner class, the
[WallpaperService.Engine() constructor](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor)
takes a reference to a
[WallpaperService](xref:Android.Service.Wallpaper.WallpaperService)
instance (compare and contrast to the Java
WallpaperService.Engine() constructor, which takes no parameters).

An example derivation of an inner class is CubeWallpaper.CubeEngine:

```csharp
class CubeWallpaper : WallpaperService {
    public override WallpaperService.Engine OnCreateEngine ()
    {
        return new CubeEngine (this);
    }

    class CubeEngine : WallpaperService.Engine {
        public CubeEngine (CubeWallpaper s)
                : base (s)
        {
        }
    }
}
```

Note how `CubeWallpaper.CubeEngine` is nested within `CubeWallpaper`,
`CubeWallpaper` inherits from the containing class of
`WallpaperService.Engine`, and `CubeWallpaper.CubeEngine` has a
constructor which takes the declaring type -- `CubeWallpaper` in this
case -- all as specified above.

### Interfaces

Java interfaces can contain three sets of members, two of which cause
problems from C#:

1. Methods

1. Types

1. Fields

Java interfaces are translated into two types:

1. An (optional) interface containing method declarations. This
   interface has the same name as the Java interface, *except* it also
   has an ' *I* ' prefix.

1. An (optional) static class containing any fields declared within the
   Java interface.

Nested types are "relocated" to be siblings of the enclosing interface
instead of nested types, with the enclosing interface name as a prefix.

For example, consider the
[android.os.Parcelable](xref:Android.OS.Parcelable) interface.
The *Parcelable* interface contains methods, nested types, and
constants. The *Parcelable* interface methods are placed into the
[Android.OS.IParcelable](xref:Android.OS.IParcelable) interface.
The *Parcelable* interface constants are placed into the
[Android.OS.ParcelableConsts](xref:Android.OS.ParcelableConsts)
type. The nested
[android.os.Parcelable.ClassLoaderCreator\<T>](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html)
and
[android.os.Parcelable.Creator\<T>](https://developer.android.com/reference/android/os/Parcelable.Creator.html)
types are currently not bound due to limitations in our generics
support; if they were supported, they would be present as the
*Android.OS.IParcelableClassLoaderCreator* and
*Android.OS.IParcelableCreator* interfaces. For example, the nested
[android.os.IBinder.DeathRecipient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html)
interface is bound as the
[Android.OS.IBinderDeathRecipient](xref:Android.OS.IBinderDeathRecipient)
interface.

> [!NOTE]
> Beginning with Xamarin.Android 1.9, Java interface
> constants are _duplicated_ in an effort to simplify porting Java
> code. This helps to improve porting Java code that relies on
> [android provider](https://developer.android.com/reference/android/provider/package-summary.html)
> interface constants.

In addition to the above types, there are four further changes:

1. A type with the same name as the Java interface is generated to
   contain constants.

1. Types containing interface constants also contain all constants that
   come from implemented Java interfaces.

1. All classes that implement a Java interface containing constants get
   a new nested InterfaceConsts type which contains constants from all
   implemented interfaces.

1. The *Consts* type is now obsolete.

For the *android.os.Parcelable* interface, this means that there will
now be an
[*Android.OS.Parcelable*](xref:Android.OS.Parcelable) type to
contain the constants. For example, the
[Parcelable.CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR)
constant will be bound as the
[*Parcelable.ContentsFileDescriptor*](xref:Android.OS.Parcelable.ContentsFileDescriptor)
constant, instead of as the *ParcelableConsts.ContentsFileDescriptor*
constant.

For interfaces containing constants which implement other interfaces
containing yet more constants, the union of all constants is now
generated. For example, the
[android.provider.MediaStore.Video.VideoColumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html)
interface implements the
[android.provider.MediaStore.MediaColumns](xref:Android.Provider.MediaStore.MediaColumns)
interface. However, prior to 1.9, the
[Android.Provider.MediaStore.Video.VideoColumnsConsts](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts)
type has no way of accessing the constants declared on
[Android.Provider.MediaStore.MediaColumnsConsts](xref:Android.Provider.MediaStore.MediaColumnsConsts).
As a result, the Java expression *MediaStore.Video.VideoColumns.TITLE*
needs to be bound to the C# expression
*MediaStore.Video.MediaColumnsConsts.Title* which is hard to discover
without reading lots of Java documentation. In 1.9, the equivalent C#
expression will be
[*MediaStore.Video.VideoColumns.Title*](xref:Android.Provider.MediaStore.Video.VideoColumns.Title).

Furthermore, consider the
[android.os.Bundle](xref:Android.OS.Bundle)
type, which implements the Java *Parcelable* interface. Since it
implements the interface, all constants on that interface are
accessible "through" the Bundle type, e.g.
*Bundle.CONTENTS_FILE_DESCRIPTOR* is a perfectly valid Java expression.
Previously, to port this expression to C# you would need to
look at all the interfaces which are implemented to see from which type
the *CONTENTS_FILE_DESCRIPTOR* came from. Starting in Xamarin.Android
1.9, classes implementing Java interfaces which contain constants will
have a nested *InterfaceConsts* type, which will contain all the
inherited interface constants. This will allow translating
*Bundle.CONTENTS_FILE_DESCRIPTOR* to
[*Bundle.InterfaceConsts.ContentsFileDescriptor*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor).

Finally, types with a *Consts* suffix such as
*Android.OS.ParcelableConsts* are now Obsolete, other than the newly
introduced InterfaceConsts nested types. They will be removed in
Xamarin.Android 3.0.

## Resources

Images, layout descriptions, binary blobs and string dictionaries can
be included in your application as
[resource files](https://developer.android.com/guide/topics/resources/providing-resources.html).
Various Android APIs are designed to
[operate on the resource IDs](https://developer.android.com/guide/topics/resources/accessing-resources.html)
instead of dealing with images, strings or binary blobs directly.

For example, a sample Android app that contains a user interface layout
( `main.axml`), an internationalization table string ( `strings.xml`)
and some icons ( `drawable-*/icon.png`) would keep its resources in the
"Resources" directory of the application:

```
Resources/
    drawable-hdpi/
        icon.png

    drawable-ldpi/
        icon.png

    drawable-mdpi/
        icon.png

    layout/
        main.axml

    values/
        strings.xml
```

The native Android APIs do not operate directly with filenames, but
instead operate on resource IDs. When you compile an Android
application that uses resources, the build system will package the
resources for distribution and generate a class called `Resource` that
contains the tokens for each one of the resources included. For
example, for the above Resources layout, this is what the R class would
expose:

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

You would then use `Resource.Drawable.icon` to reference the
`drawable/icon.png` file, or `Resource.Layout.main` to reference the
`layout/main.xml` file, or `Resource.String.first_string` to reference
the first string in the dictionary file `values/strings.xml`.

## Constants and Enumerations

The native Android APIs have many methods that take or return an int
that must be mapped to a constant field to determine what the int
means. To use these methods, the user is required to consult
the documentation to see which constants are appropriate values, which
is less than ideal.

For example, consider
[Activity.requestWindowFeature(int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

In these cases, we endeavor to group related constants together into a
.NET enumeration, and remap the method to take the enumeration instead.
By doing this, we are able to offer IntelliSense selection of the
potential values.

The above example becomes:
[Activity.RequestWindowFeature(WindowFeatures featureId)](xref:Android.App.Activity.RequestWindowFeature*).

Note that this is a very manual process to figure out which constants
belong together, and which APIs consume these constants. Please file
bugs for any constants used in the API that would be better expressed as
an enumeration.