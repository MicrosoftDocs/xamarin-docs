---
title: "Binding a Java Library"
description: "The Android community has many Java libraries that you may want to use in your app; this guide explains how to incorporate Java libraries into your Xamarin.Android application by creating a Bindings Library."
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
---

# Binding a Java Library

_The Android community has many Java libraries that you may want to use in your app; this guide explains how to incorporate Java libraries into your Xamarin.Android application by creating a Bindings Library._

## Overview

The third-party library ecosystem for Android is massive. Because of this, it frequently makes sense to use an existing Android library than to create a new one. Xamarin.Android offers two ways to use these libraries:

-   Create a *Bindings Library* that automatically wraps the library with C# wrappers so you can invoke Java code via C# calls.

-   Use the *Java Native Interface* (*JNI*) to invoke calls in Java library code directly. JNI is a programming framework that enables Java code to call and be called by native applications or libraries.

This guide explains the first option: how to create a *Bindings Library* that wraps one or more existing Java libraries into an assembly that you can link to in your application. For more information about using
JNI, see [Working with JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implements bindings by using *Managed Callable Wrappers* (*MCW*). MCW is a JNI bridge that is used when managed code needs to invoke Java code. Managed callable wrappers also provide support for subclassing Java types and for overriding virtual methods on Java types. Likewise, whenever Android runtime (ART) code wishes to invoke managed code, it does so via another JNI bridge known as Android Callable Wrappers (ACW). This [architecture](~/android/internals/architecture.md) is illustrated in the following diagram:

[![Android JNI bridge architecture](images/architecture.png)](images/architecture.png#lightbox)

A Bindings Library is an assembly containing Managed Callable Wrappers for Java types. For example, here is a Java type, `MyClass`, that we want to wrap in a Bindings Library:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

After we generate a Bindings Library for the **.jar** that contains `MyClass`, we can instantiate it and call methods on it from C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

To create this Bindings Library, you use the Xamarin.Android *Java Bindings Library* template. The resulting binding project creates a .NET assembly with the MCW classes, **.jar** file(s), and resources for Android Library projects embedded in it. You can also create Bindings Libraries for Android Archive (.AAR) files and Eclipse Android Library projects. By referencing the resulting Bindings Library DLL assembly, you can reuse an existing Java library in your Xamarin.Android project.

When you reference types in your Binding Library, you must use the namespace of your binding library. Typically, you add a `using` directive at the top of your C# source files that is the .NET namespace version of the Java package name. For example, if the Java package name for your bound **.jar** is the following:

```csharp
com.company.package
```

Then you would put the following `using` statement at the top of your C# source files to access types in the bound **.jar** file:

```csharp
using Com.Company.Package;
```


When binding an existing Android library, it is necessary to keep the following points in mind:

* **Are there any external dependencies for the library?** &ndash; Any Java dependencies required by the Android library must be included in the Xamarin.Android project as a **ReferenceJar** or as an **EmbeddedReferenceJar**. Any native assemblies must be added to the binding project as an **EmbeddedNativeLibrary**.  

* **What version of the Android API is does the Android library target?** &ndash; It is not possible to "downgrade" the Android API level; ensure that the Xamarin.Android binding project is targeting the same API level (or higher) as the Android library.

* **What version of the JDK was used to compile the library?** &ndash; Binding errors may occur if the Android library was built with a different version of JDK than in use by Xamarin.Android. If possible, recompile the Android library using the same version of the JDK that is used by your installation of Xamarin.Android.


## Build Actions

When you create a Bindings Library, you set *build actions* on the **.jar** or .AAR files that you incorporate into your Bindings Library project &ndash; each build action determines how the **.jar** or .AAR file will be embedded into (or referenced by) your Bindings Library. The following
list summarizes these build actions:

* `EmbeddedJar` &ndash; Embeds the **.jar** into the resulting Bindings Library DLL as an embedded resource. This is the simplest and most commonly-used build action. Use this option when you want the **.jar** automatically compiled into byte code and packaged into the Bindings Library.

* `InputJar` &ndash; Does not embed the **.jar** into the resulting Bindings Library .DLL. Your Bindings Library .DLL will have a dependency on this **.jar** at runtime. Use this option when you do not want to include the **.jar** in your Bindings Library (for example, for licensing reasons). If you use this option, you must ensure that the input **.jar** is available on the device that runs your app.

* `LibraryProjectZip` &ndash; Embeds an .AAR file into the resulting Bindings Library .DLL. This is similar to EmbeddedJar, except that you can access resources (as well as code) in the bound .AAR file. Use this option when you want to embed an .AAR into your Bindings Library.

* `ReferenceJar` &ndash; Specifies a reference **.jar**: a reference **.jar** is a **.jar** that one of your bound **.jar** or .AAR files depends on. This reference **.jar** is used only to satisfy compile-time dependencies. When you use this build action, C# bindings are not created for the reference **.jar** and it is not embedded in the resulting Bindings Library .DLL. Use this option when you will make a Bindings Library for the reference **.jar** but have not done so yet. This build action is useful for packaging multiple **.jar**s (and/or .AARs) into multiple interdependent Bindings Libraries.

* `EmbeddedReferenceJar` &ndash; Embeds a reference **.jar** into the resulting Bindings Library .DLL. Use this build action when you want to create C# bindings for both the input **.jar** (or .AAR) and all of its reference **.jar**(s) in your Bindings Library.

* `EmbeddedNativeLibrary` &ndash; Embeds a native **.so** into the binding. This build action is used for **.so** files that are required by the **.jar** file being bound. It may be necessary to manually load the **.so** library before executing code from the Java library. This is described below.

These build actions are explained in more detail in the following guides.

Additionally, the following build actions are used to help importing Java API documentation and convert them into C# XML documentation:

* `JavaDocJar` is used to point to Javadoc archive Jar for a Java library that conforms to a Maven package style (usually `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` is used to point to `index.html` file within the API reference documentation HTML.
* `JavaSourceJar` is used to complement `JavaDocJar`, to first generate JavaDoc from sources and then treat the results as `JavaDocIndex`, for a Java library that conforms to a Maven package style (usually `FOOBAR-sources**.jar**`).

The API documentation should be the default doclet from Java8, Java7 or Java6 SDK (they are all different format), or the DroidDoc style.

## Including a Native Library in a Binding

It may be necessary to include a **.so** library in a Xamarin.Android binding project as a part of binding a Java library. When the wrapped Java code executes, Xamarin.Android will fail to make the JNI call and the error message _java.lang.UnsatisfiedLinkError: Native method not found:_ will appear in the logcat out for the application.

The fix for this is to manually load the **.so** library with a call to `Java.Lang.JavaSystem.LoadLibrary`. For example assuming that a Xamarin.Android project has shared library **libpocketsphinx_jni.so** included in the binding project with a build action of **EmbeddedNativeLibrary**, the following snippet (executed before using the shared library) will load the **.so** library:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## Adapting Java APIs to C&eparsl;

The Xamarin.Android Binding Generator will change some Java idioms and patterns to correspond to .NET patterns. The following list describes how Java is mapped to C#/.NET:

-   _Setter/Getter methods_ in Java are _Properties_ in .NET.

-   _Fields_ in Java are _Properties_ in .NET.

-   _Listeners/Listener Interfaces_ in Java are _Events_ in .NET. The parameters of methods in the callback interfaces will be represented by an `EventArgs` subclass.

-   A _Static Nested class_ in Java is a _Nested class_ in .NET.

-   An _Inner class_ in Java is a _Nested class_ with an instance constructor in C#.



## Binding Scenarios

The following binding scenario guides can help you bind a Java library (or libraries) for incorporation into your app:

-   [Binding a .JAR](~/android/platform/binding-java-library/binding-a-jar.md)
    is a walkthrough for creating Bindings Libraries for **.jar** files.

-   [Binding an .AAR](~/android/platform/binding-java-library/binding-an-aar.md) is a walkthrough for creating Bindings Libraries for .AAR files. Read this walkthrough to learn how to bind Android Studio libraries.

-   [Binding an Eclipse Library Project](~/android/platform/binding-java-library/binding-a-library-project.md) is a walkthrough for creating binding libraries from Android Library Projects. Read this walkthrough to learn how to bind Eclipse Android Library Projects.

-   [Customizing Bindings](~/android/platform/binding-java-library/customizing-bindings/index.md) explains how to make manual modifications to the binding to resolve build errors and shape the resulting API so that it is more "C#-like".

-   [Troubleshooting Bindings](~/android/platform/binding-java-library/troubleshooting-bindings.md) lists common binding error scenarios, explains possible causes, and offers suggestions for resolving these errors.


## Related Links

- [Working with JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Using Native Libraries](~/android/platform/native-libraries.md)
