---
title: "Xamarin for Java developers"
description: "If you are a Java developer, you are well on your way to leveraging your skills and existing code on the Xamarin platform while reaping the code reuse benefits of C#. You will find that C# syntax is very similar to Java syntax, and that both languages provide very similar features. In addition, you'll discover features unique to C# that will make your development life easier."
ms.service: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
---

# Xamarin for Java developers

_If you are a Java developer, you are well on your way to leveraging your skills and existing code on the Xamarin platform while reaping the code reuse benefits of C#. You will find that C# syntax is very similar to Java syntax, and that both languages provide very similar features. In addition, you'll discover features unique to C# that will make your development life easier._

## Overview

This article provides an introduction to C# programming for Java
developers, focusing primarily on the C# language features that you
will encounter while developing Xamarin.Android applications. Also,
this article explains how these features differ from their Java
counterparts, and it introduces important C# features (relevant to
Xamarin.Android) that are not available in Java. Links to additional
reference material are included, so you can use this article as a
"jumping off" point for further study of C# and .NET.

If you are familiar with Java, you will feel instantly at home with the
syntax of C#. C# syntax is very similar to Java syntax &ndash; C# is a
"curly brace" language like Java, C, and C++. In many ways, C# syntax
reads like a superset of Java syntax, but with a few renamed and added
keywords.

Many key characteristics of Java can be found in C#:

- Class-based object-oriented programming

- Strong typing

- Support for interfaces

- Generics

- Garbage collection

- Runtime compilation

Both Java and C# are compiled to an intermediate language that is run
in a managed execution environment. Both C# and Java are
statically-typed, and both languages treat strings as immutable types.
Both languages use a single-rooted class hierarchy. Like Java, C#
supports only single inheritance and does not allow for global methods.
In both languages, objects are created on the heap using the `new`
keyword, and objects are garbage-collected when they are no longer
used. Both languages provide formal exception handling support with
`try`/`catch` semantics. Both provide thread management and
synchronization support.

However, there are many differences between Java and C#. For example:

- In Java, you can pass parameters only by value, while in C# you can
    pass by reference as well as by value. (C# provides the `ref` and
    `out` keywords for passing parameters by reference; there is no
    equivalent to these in Java).

- Java does not support preprocessor directives like `#define`.

- Java does not support unsigned integer types, while C# provides
    unsigned integer types such as `ulong`, `uint`, `ushort` and
    `byte`.

- Java does not support operator overloading; in C# you can overload
    operators and conversions.

- In a Java `switch` statement, code can fall through into the 
    next switch section, but in C# the end of every `switch` section must
    terminate the switch (the end of each section must close with a
    `break` statement).

- In Java, you specify the exceptions thrown by a method with the
    `throws` keyword, but C# has no concept of checked exceptions &ndash;
    the `throws` keyword is not supported in C#.

- C# supports Language-Integrated Query (LINQ), which lets you use
    the reserved words `from`, `select`, and `where` to write queries
    against collections in a way that is similar to database queries.

Of course, there are many more differences between C# and Java than can
be covered in this article. Also, both Java and C# continue to
evolve (for example, Java 8, which is not yet in the Android toolchain,
supports C#-style lambda expressions) so these differences will change over
time. Only the most important differences currently encountered by Java
developers new to Xamarin.Android are outlined here.

- [Going from Java to C# Development](#fundamentals) provides an
    introduction to the fundamental differences between C# and Java.

- [Object-Oriented Programming Features](#oopfeatures) outlines
    the most important object-oriented feature differences between
    the two languages.

- [Keyword Differences](#keywords) provides a table of useful
    keyword equivalents, C#-only keywords, and links to C# keyword
    definitions.

C# brings many key features to Xamarin.Android that are not currently
readily available to Java developers on Android. These features can
help you to write better code in less time:

- [Properties](#properties) &ndash; With C#'s property system, you can access
    member variables safely and directly without having to write setter
    and getter methods.

- [Lambda Expressions](#lambdas) &ndash; In C# you can use anonymous methods (also
    called *lambdas*) to express your functionality more succinctly and
    more efficiently. You can avoid the overhead of having to write
    one-time-use objects, and you can pass local state to a method
    without having to add parameters.

- [Event Handling](#events) &ndash; C# provides language-level support for
    *event-driven programming*, where an object can register to be
    notified when an event of interest occurs. The `event` keyword
    defines a multicast broadcast mechanism that a publisher
    class can use to notify event subscribers.

- [Asynchronous Programming](#async) &ndash; The asynchronous
    programming features of C# (`async`/`await`) keep apps responsive.
    The language-level support of this feature makes async programming
    easy to implement and less error-prone.

Finally, Xamarin allows you to
[leverage existing Java assets](#interop) via a technology known as
*binding*. You can call your existing Java code, frameworks, and
libraries from C# by making use of Xamarin's automatic binding
generators. To do this, you simply create a static library in Java and
expose it to C# via a binding.

> [!NOTE]
> Android programming uses a specific version of the Java language
> that supports all Java 7 features [and a subset of Java 8](https://developer.android.com/studio/write/java8-support.html).
>
> Some features mentioned on this page (such as the `var` keyword in C#) are
> available in newer versions of Java (e.g. [`var` in Java 10](https://developer.oracle.com/java/jdk-10-local-variable-type-inference.html)), but are still not available to Android developers.

<a name="fundamentals"></a>

## Going from Java to C# development

The following sections outline the basic "getting started" differences
between C# and Java; a later section describes the object-oriented
differences between these languages.

### Libraries vs. assemblies

Java typically packages related classes in **.jar** files. In C# and
.NET, however, reusable bits of precompiled code are packaged into
*assemblies*, which are typically packaged as *.dll* files. An assembly
is a unit of deployment for C#/.NET code, and each assembly is
typically associated with a C# project. Assemblies contain intermediate
code (IL) that is just-in-time compiled at runtime.

For more information about assemblies, see the
[Assemblies and the Global Assembly Cache](/dotnet/csharp/programming-guide/concepts/assemblies-gac/)
topic.

### Packages vs. namespaces

C# uses the `namespace` keyword to group related types together; this
is similar to Java's `package` keyword. Typically, a Xamarin.Android
app will reside in a namespace created for that app. For example, the
following C# code declares the `WeatherApp` namespace wrapper for a
weather-reporting app:

```csharp
namespace WeatherApp
{
    ...
```

### Importing types

When you make use of types defined in external namespaces, you import
these types with a `using` statement (which is very similar to the Java
`import` statement). In Java, you might import a single type with a
statement like the following:

```java
import javax.swing.JButton
```

You might import an entire Java package with a statement like this:

```java
import javax.swing.*
```

The C# `using` statement works in a very similar way, but it allows you
to import an entire package without specifying a wildcard. For example,
you will often see a series of `using` statements at the beginning of
Xamarin.Android source files, as seen in this example:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

These statements import functionality from the `System`, `Android.App`,
`Android.Content`, etc. namespaces.

### Generics

Both Java and C# support *generics*, which are placeholders that let
you plug in different types at compile time. However, generics work
slightly differently in C#. In Java,
[type erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)
makes type information available only at compile time, but not at run time. By
contrast, the .NET common language runtime (CLR) provides explicit
support for generic types, which means that C# has access to type
information at runtime. In day-to-day Xamarin.Android development, the
importance of this distinction is not often apparent, but if you 
are using [reflection](/dotnet/csharp/programming-guide/concepts/reflection),
you will depend on this feature to access type information at
run time.

In Xamarin.Android, you will often see the generic method
`FindViewById` used to get a reference to a layout control. This
method accepts a generic type parameter that specifies the type of
control to look up. For example:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

In this code example, `FindViewById` gets a reference to the `TextView`
control that is defined in the layout as **Label**, then returns it as
a `TextView` type.

For more information about generics, see the 
[Generics](/dotnet/csharp/programming-guide/generics/index) topic.
Note that there are some limitations in Xamarin.Android support for
generic C# classes; for more information, see
[Limitations](~/android/internals/limitations.md).

<a name="oopfeatures"></a>

## Object-oriented programming features

Both Java and C# use very similar object-oriented programming idioms:

- All classes are ultimately derived from a single root object
    &ndash; all Java objects derive from `java.lang.Object`, while all
    C# objects derive from `System.Object`.

- Instances of classes are reference types.

- When you access the properties and methods of an instance, you
    use the "`.`" operator.

- All class instances are created on the heap via the `new` operator.

- Because both languages use garbage collection, there is no way to
    explicitly release unused objects (i.e., there is not a `delete`
    keyword as there is in C++).

- You can extend classes through inheritance, and both languages
    only allow a single base class per type.

- You can define interfaces, and a class can inherit from (i.e.,
    implement) multiple interface definitions.

However, there are also some important differences:

- Java has two powerful features that C# does not support: anonymous
    classes and inner classes. (However, C# does allow nesting of class
    definitions &ndash; C#'s nested classes are like Java's static
    nested classes.)

- C# supports C-style structure types (`struct`) while Java does not.

- In C#, you can implement a class definition in separate source
    files by using the `partial` keyword.

- C# interfaces cannot declare fields.

- C# uses C++-style destructor syntax to express finalizers. The
    syntax is different from Java's `finalize` method, but the
    semantics are nearly the same. (Note that in C#, destructors
    automatically call the base-class destructor &ndash; in contrast to
    Java where an explicit call to `super.finalize` is used.)

### Class inheritance

To extend a class in Java, you use the `extends` keyword. To extend a
class in C#, you use a colon (`:`) to indicate derivation. For example,
in Xamarin.Android apps, you will often see class derivations that
resemble the following code fragment:

```csharp
public class MainActivity : Activity
{
    ...
```

In this example, `MainActivity` inherits from the `Activity` class.

To declare support for an interface in Java, you use the `implements`
keyword. However, in C#, you simply add interface names to the list of
classes to inherit from, as shown in this code fragment:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

In this example, `SensorsActivity` inherits from `Activity` and
implements the functionality declared in the `ISensorEventListener`
interface. Note that the list of interfaces must come after the base
class (or you will get a compile-time error). By convention, C#
interface names are prepended with an upper-case "I"; this makes it
possible to determine which classes are interfaces without requiring an
`implements` keyword.

When you want to prevent a class from being further subclassed in C#,
you precede the class name with `sealed` &ndash; in Java, you precede the
class name with `final`.

For more about C# class definitions, see the
[Classes](/dotnet/csharp/programming-guide/classes-and-structs/classes) and
[Inheritance](/dotnet/csharp/programming-guide/classes-and-structs/inheritance) topics.

### Properties

In Java, mutator methods (setters) and inspector methods (getters) are
often used to control how changes are made to class members while
hiding and protecting these members from outside code. For example, the
Android `TextView` class provides `getText` and `setText` methods. C#
provides a similar but more direct mechanism known as *properties*.
Users of a C# class can access a property in the same way as they would
access a field, but each access actually results in
a method call that is transparent to the caller. This "under the
covers" method can implement side effects such as setting other values,
performing conversions, or changing object state.

Properties are often used for accessing and modifying UI (user
interface) object members. For example:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

In this example, width and height values are read from the `rulerView`
object by accessing its `MeasuredWidth` and `MeasuredHeight`
properties. When these properties are read, values from their
associated (but hidden) field values are fetched behind the scenes and
returned to the caller. The `rulerView` object may store width and
height values in one unit of measurement (say, pixels) and convert
these values on-the-fly to a different unit of measurement (say,
millimeters) when the `MeasuredWidth` and `MeasuredHeight` properties
are accessed.

The `rulerView` object also has a property called `DrawingCacheEnabled`
&ndash; the example code sets this property to `true` to 
enable the drawing cache in `rulerView`. Behind the scenes, an
associated hidden field is updated with the new value, and possibly
other aspects of `rulerView` state are modified. For example, when
`DrawingCacheEnabled` is set to `false`, `rulerView` may also erase
any drawing cache information already accumulated in the object.

Access to properties can be read/write, read-only, or write-only. Also,
you can use different access modifiers for reading and writing. For
example, you can define a property that has public read access 
but private write access.

For more information about C# properties, see the
[Properties](/dotnet/csharp/programming-guide/classes-and-structs/properties)
topic.

### Calling base class methods

To call a base-class constructor in C#, you use a colon (`:`) followed
by the `base` keyword and an initializer list; this `base` constructor
call is placed immediately after the derived constructor parameter
list. The base-class constructor is called on entry to the derived
constructor; the compiler inserts the call to the base constructor at
the start of the method body. The following code fragment illustrates a
base constructor called from a derived constructor in a Xamarin.Android
app:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

In this example, the `PictureLayout` class is derived from the
`ViewGroup` class. The `PictureLayout` constructor shown in this
example accepts a `context` argument and passes it to the `ViewGroup`
constructor via the `base(context)` call.

To call a base-class method in C#, use the `base` keyword. For example,
Xamarin.Android apps often make calls to base methods as shown here:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

In this case, the `OnCreate` method defined by the derived class
(`MainActivity`) calls the `OnCreate` method of the base class
(`Activity`).

### Access modifiers

Java and C# both support the `public`, `private`, and `protected`
access modifiers. However, C# supports two additional access modifiers:

- **`internal`** &ndash; The class member is accessible only within
    the current assembly.

- **`protected internal`** &ndash; The class member is accessible
    within the defining assembly, the defining class, and derived
    classes (derived classes both inside and outside the assembly
    have access).

For more information about C# access modifiers, see the
[Access Modifiers](/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)
topic.

### Virtual and override methods

Both Java and C# support *polymorphism*, the ability to treat related
objects in the same manner. In both languages, you can use a base-class
reference to refer to a derived-class object, and the methods of
a derived class can override the methods of its base classes. Both
languages have the concept of a *virtual* method, a method in a base
class that is designed to be replaced by a method in a derived class.
Like Java, C# supports `abstract` classes and methods.

However, there are some differences between Java and C# in how you
declare virtual methods and override them:

- In C#, methods are non-virtual by default. Parent classes must
    explicitly label which methods are to be overridden by using the
    `virtual` keyword. By contrast, all methods in Java are virtual
    methods by default.

- To prevent a method from being overridden in C#, you simply leave
    off the `virtual` keyword. By contrast, Java uses the `final`
    keyword to mark a method with "override is not allowed."

- C# derived classes must use the `override` keyword to explicitly
    indicate that a virtual base-class method is being overridden.

For more information about C#'s support for polymorphism, see the
[Polymorphism](/dotnet/csharp/programming-guide/classes-and-structs/polymorphism)
topic.

<a name="lambdas"></a>

## Lambda expressions

C# makes it possible to create *closures*: inline, anonymous methods
that can access the state of the method in which they are enclosed.
Using lambda expressions, you can write fewer lines of code to
implement the same functionality that you might have implemented in
Java with many more lines of code.

Lambda expressions make it possible for you to skip the extra ceremony
involved in creating a one-time-use class or anonymous class as you
would in Java &ndash; instead, you can just write the business logic of
your method code inline. Also, because lambdas have access to the
variables in the surrounding method, you don't have to create a long
parameter list to pass state to your method code.

In C#, lambda expressions are created with the `=>` operator as
shown here:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

In Xamarin.Android, lambda expressions are often used for defining
event handlers. For example:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

In this example, the lambda expression code (the code within the curly
braces) increments a click count and updates the `button` text to
display the click count. This lambda expression is registered with the
`button` object as a click event handler to be called whenever the
button is tapped. (Event handlers are explained in more detail below.)
In this simple example, the `sender` and `args` parameters are not used
by the lambda expression code, but they are required in the lambda
expression to meet the method signature requirements for event
registration. Under the hood, the C# compiler translates the lambda
expression into an anonymous method that is called whenever button
click events take place.

For more information about C# and lambda expressions, see the
[Lambda Expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)
topic.

<a name="events"></a>

## Event handling

An *event* is a way for an object to notify registered subscribers when
something interesting happens to that object. Unlike in Java, where a
subscriber typically implements a `Listener` interface that contains a
callback method, C# provides language-level support for event handling
through *delegates*. A *delegate* is like an object-oriented type-safe
function pointer &ndash; it encapsulates an object reference and a
method token. If a client object wants to subscribe to an event, it
creates a delegate and passes the delegate to the notifying object.
When the event occurs, the notifying object invokes the method
represented by the delegate object, notifying the subscribing client
object of the event. In C#, event handlers are essentially nothing more
than methods that are invoked through delegates.

For more information about delegates, see the
[Delegates](/dotnet/csharp/programming-guide/delegates/index) topic.

In C#, events are *multicast*; that is, more than one listener can be
notified when an event takes place. This difference is observed when
you consider the syntactical differences between Java and C# event
registration. In Java you call `SetXXXListener` to register for event
notifications; in C# you use the `+=` operator to register for event
notifications by "adding" your delegate to the list of event listeners.
In Java, you call `SetXXXListener` to unregister, while in C# you use
the `-=` to "subtract" your delegate from the list of listeners.

In Xamarin.Android, events are often used for notifying objects
when a user does something to a UI control. Normally, a UI control will
have members that are defined using the `event` keyword; you attach
your delegates to these members to subscribe to events from
that UI control.

To subscribe to an event:

1. Create a delegate object that refers to the method that you want
    to be invoked when the event occurs.

2. Use the `+=` operator to attach your delegate to the event
    you are subscribing to.

The following example defines a delegate (with an explicit
use of the `delegate` keyword) to subscribe to button clicks.
This button-click handler starts a new activity:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

However, you also can use a lambda expression to register for events,
skipping the `delegate` keyword altogether. For example:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

In this example, the `startActivityButton` object has an event that
expects a delegate with a certain method signature: one that accepts
sender and event arguments and returns void. However, because we don't
want to go to the trouble to explicitly define such a delegate or its
method, we declare the signature of the method with `(sender, e)` and
use a lambda expression to implement the body of the event handler.
Note that we have to declare this parameter list even though we 
aren't using the `sender` and `e` parameters.

It is important to remember that you can unsubscribe a delegate (via
the `-=` operator), but you cannot unsubscribe a lambda expression &ndash;
attempting to do so can cause memory leaks. Use the lambda form of
event registration only when your handler will not unsubscribe from the
event.

Typically, lambda expressions are used to declare event handlers in
Xamarin.Android code. This shorthand way to declare event handlers may
seem cryptic at first, but it saves an enormous amount of time when you
are writing and reading code. With increasing familiarity, you become
accustomed to recognizing this pattern (which occurs frequently in
Xamarin.Android code), and you spend more time thinking about the
business logic of your application and less time wading through
syntactical overhead.

<a name="async"></a>

## Asynchronous programming

*Asynchronous programming* is a way to improve the overall
responsiveness of your application. Asynchronous programming features
make it possible for the rest of your app code to continue running
while some part of your app is blocked by a lengthy operation.
Accessing the web, processing images, and reading/writing files are
examples of operations that can cause an entire app to appear to freeze
up if it is not written asynchronously.

C# includes language-level support for asynchronous programming via the
`async` and `await` keywords. These language features make it very easy
to write code that performs long-running tasks without blocking the
main thread of your application. Briefly, you use the `async` keyword
on a method to indicate that the code in the method is to run
asynchronously and not block the caller's thread. You use the `await`
keyword when you call methods that are marked with `async`. The
compiler interprets the `await` as the point where your method
execution is to be moved to a background thread (a task is returned
to the caller). When this task completes, execution of the code resumes
on the caller's thread at the `await` point in your code, returning the
results of the `async` call. By convention, methods that run
asynchronously have `Async` suffixed to their names.

In Xamarin.Android applications, `async` and `await` are typically used
to free up the UI thread so that it can respond to user input (such as the
tapping of a **Cancel** button) while a long-running operation takes
place in a background task.

In the following example, a button click event handler causes an asynchronous
operation to download an image from the web:

```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

In this example, when the user clicks the `downloadButton` control, the
`downloadAsync` event handler creates a `WebClient` object and a `Uri`
object to fetch an image from the specifed URL. Next, it calls the
`WebClient` object's `DownloadDataTaskAsync` method with this URL to
retrieve the image.

Notice that the method declaration of `downloadAsync` is prefaced by
the `async` keyword to indicate that it will run asynchronously and
return a task. Also note that the call to `DownloadDataTaskAsync` is
preceded by the `await` keyword. The app moves the execution of the
event handler (starting at the point where `await` appears) to a
background thread until `DownloadDataTaskAsync` completes and returns.
Meanwhile, the app's UI thread can still respond to user input and fire
event handlers for the other controls. When `DownloadDataTaskAsync`
completes (which may take several seconds), execution resumes where the
`bytes` variable is set to the result of the call to
`DownloadDataTaskAsync`, and the remainder of the event handler code
displays the downloaded image on the caller's (UI) thread.

For an introduction to `async`/`await` in C#, see the
[Asynchronous Programming with Async and Await](/dotnet/csharp/async) topic.
For more information about Xamarin support of asynchronous programming
features, see
[Async Support Overview](~/cross-platform/platform/async.md).

<a name="keywords"></a>

## Keyword differences

Many language keywords used in Java are also used in C#. There are also
a number of Java keywords that have an equivalent but differently-named
counterpart in C#, as listed in this table:

|Java|C#|Description|
|---|---|---|
|`boolean`|[bool](/dotnet/csharp/language-reference/keywords/bool)|Used for declaring the boolean values true and false.|
|`extends`|`:`|Precedes the class and interfaces to inherit from.|
|`implements`|`:`|Precedes the class and interfaces to inherit from.|
|`import`|[using](/dotnet/csharp/language-reference/keywords/using)|Imports types from a namespace, also used for creating a namespace alias.|
|`final`|[sealed](/dotnet/csharp/language-reference/keywords/sealed)|Prevents class derivation; prevents methods and properties from being overridden in derived classes.|
|`instanceof`|[is](/dotnet/csharp/language-reference/keywords/is)|Evaluates whether an object is compatible with a given type.|
|`native`|[extern](/dotnet/csharp/language-reference/keywords/extern)|Declares a method that is implemented externally.|
|`package`|[namespace](/dotnet/csharp/language-reference/keywords/namespace)|Declares a scope for a related set of objects.|
|`T...`|[params T](/dotnet/csharp/language-reference/keywords/params)|Specifies a method parameter that takes a variable number of arguments.|
|`super`|[base](/dotnet/csharp/language-reference/keywords/base)|Used to access members of the parent class from within a derived class.|
|`synchronized`|[lock](/dotnet/csharp/language-reference/keywords/lock-statement)|Wraps a critical section of code with lock acquisition and release.|

Also, there are many keywords that are unique to C# and have no
counterpart in the Java used on Android. Xamarin.Android code often makes use of the
following C# keywords (this table is useful to refer to when you
are reading through Xamarin.Android
[sample code](/samples/browse/?products=xamarin&term=Xamarin.Android)):

|C#|Description|
|---|---|
|[as](/dotnet/csharp/language-reference/keywords/as)|Performs conversions between compatible reference types or nullable types.|
|[async](/dotnet/csharp/language-reference/keywords/async)|Specifies that a method or lambda expression is asynchronous.|
|[await](/dotnet/csharp/language-reference/keywords/await)|Suspends the execution of a method until a task completes.|
|[byte](/dotnet/csharp/language-reference/keywords/byte)|Unsigned 8-bit integer type.|
|[delegate](/dotnet/csharp/language-reference/keywords/delegate)|Used to encapsulate a method or anonymous method.|
|[enum](/dotnet/csharp/language-reference/keywords/enum)|Declares an enumeration, a set of named constants.|
|[event](/dotnet/csharp/language-reference/keywords/event)|Declares an event in a publisher class.|
|[fixed](/dotnet/csharp/language-reference/keywords/fixed-statement)|Prevents a variable from being relocated.|
|`get`|Defines an accessor method that retrieves the value of a property.|
|[in](/dotnet/csharp/language-reference/keywords/in-generic-modifier)|Enables a parameter to accept a less derived type in a generic interface.|
|[object](/dotnet/csharp/language-reference/keywords/object)|An alias for the Object type in the .NET framework.|
|[out](/dotnet/csharp/language-reference/keywords/out)|Parameter modifier or generic type parameter declaration.|
|[override](/dotnet/csharp/language-reference/keywords/override)|Extends or modifies the implementation of an inherited member.|
|[partial](/dotnet/csharp/language-reference/keywords/partial-method)|Declares a definition to be split into multiple files, or splits a method definition from its implementation.|
|[readonly](/dotnet/csharp/language-reference/keywords/readonly)|Declares that a class member can be assigned only at declaration time or by the class constructor.|
|[ref](/dotnet/csharp/language-reference/keywords/ref)|Causes an argument to be passed by reference rather than by value.|
|[set](/dotnet/csharp/language-reference/keywords/set)|Defines an accessor method that sets the value of a property.|
|[string](/dotnet/csharp/language-reference/keywords/string)|Alias for the String type in the .NET framework.|
|[struct](/dotnet/csharp/language-reference/keywords/struct)|A value type that encapsulates a group of related variables.|
|[typeof](/dotnet/csharp/language-reference/keywords/typeof)|Obtains the type of an object.|
|[var](/dotnet/csharp/language-reference/keywords/var)|Declares an implicitly-typed local variable.|
|[value](/dotnet/csharp/language-reference/keywords/value)|References the value that client code wants to assign to a property.|
|[virtual](/dotnet/csharp/language-reference/keywords/virtual)|Allows a method to be overridden in a derived class.|

<a name="interop"></a>

## Interoperating with existing java code

If you have existing Java functionality that you do not want to convert
to C#, you can reuse your existing Java libraries in Xamarin.Android
applications via two techniques:

- **Create a Java Bindings Library** &ndash; Using this approach, you use Xamarin
   tools to generate C# wrappers around Java types. These wrappers are called
   *bindings*. As a result, your Xamarin.Android application can use your *.jar*
   file by calling into these wrappers.

- **Java Native Interface** &ndash; The *Java Native Interface* (JNI) is a
   framework that makes it possible for C# apps to call or be called by
   Java code.

For more information about these techniques, see
[Java Integration Overview](~/android/platform/java-integration/index.md).

## Further reading

The MSDN [C# Programming Guide](/dotnet/csharp/programming-guide/) is a
great way to get started in learning the C# programming language, and you can use
the [C# Reference](/dotnet/csharp/language-reference/)
to look up particular C# language features.

In the same way that Java knowledge is at least as much about
familiarity with the Java class libraries as knowing the Java language,
practical knowledge of C# requires some familiarity with the .NET
framework. Microsoft's
[Moving to C# and the .NET Framework, for Java
Developers](https://www.microsoft.com/download/details.aspx?id=6073)
learning packet is a good way to learn more about the .NET framework from a Java
perspective (while gaining a deeper understanding of C#).

When you are ready to tackle your first Xamarin.Android project in C#,
our [Hello, Android](~/android/get-started/hello-android/index.md) series
can help you build your first Xamarin.Android application and further
advance your understanding of the fundamentals of Android application
development with Xamarin.

## Summary

This article provided an introduction to the Xamarin.Android C#
programming environment from a Java developer's perspective. It pointed
out the similarities between C# and Java while explaining their
practical differences. It introduced assemblies and namespaces,
explained how to import external types, and provided an overview of the
differences in access modifiers, generics, class derivation, calling
base-class methods, method overriding, and event handling. It
introduced C# features that are not available in Java, such as
properties, `async`/`await` asynchronous programming, lambdas, C#
delegates, and the C# event handling system. It included tables of
important C# keywords, explained how to interoperate with existing Java
libraries, and provided links to related documentation for further
study.

## Related links

- [Java Integration Overview](~/android/platform/java-integration/index.md)
- [C# Programming Guide](/dotnet/csharp/programming-guide/)
- [C# Reference](/dotnet/csharp/language-reference/index)
- [Moving to C# and the .NET Framework, for Java Developers](https://www.microsoft.com/download/details.aspx?id=6073)
