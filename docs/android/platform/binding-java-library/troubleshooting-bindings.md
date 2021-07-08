---
title: "Troubleshooting Bindings"
description: "This article summarizes serveral common errors that may occur when generating bindings, along with possible causes and suggested ways to resolve them."
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
---

# Troubleshooting Bindings

> [!IMPORTANT]
> We're currently investigating custom binding usage on the Xamarin platform. Please take [**this survey**](https://www.surveymonkey.com/r/KKBHNLT) to inform future development efforts.

_This article summarizes serveral common errors that may occur when generating bindings, along with possible causes and suggested ways to resolve them._

## Overview

Binding an Android library (an **.aar** or a **.jar**) file is seldom a
straightforward affair; it usually requires additional effort to
mitigate issues that result from the differences between Java and .NET.
These issues will prevent Xamarin.Android from binding the Android
library and present themselves as error messages in the build log. This
guide will provide some tips for troubleshooting the issues, list some
of the more common problems/scenarios, and provide possible solutions
to successfully binding the Android library.

When binding an existing Android library, it is necessary to keep in mind the following points:

- **The external dependencies for the library** &ndash; Any Java
  dependencies required by the Android library must be included in the
  Xamarin.Android project as a **ReferenceJar** or as an
  **EmbeddedReferenceJar**.

- **The Android API level that the Android library is targetting**
  &ndash; It is not possible to "downgrade" the Android API level;
  ensure that the Xamarin.Android binding project is targeting the same
  API level (or higher) as the Android library.

- **The version of the Android JDK that was used to package the Android
  library** &ndash; Binding errors may occur if the Android library was
  built with a different version of JDK than the one in use by
  Xamarin.Android. If possible, recompile the Android library using the
  same version of the JDK that is used by your installation of
  Xamarin.Android.

The first step to troubleshooting issues with binding a Xamarin.Android
library is to enable
[diagnostic MSBuild output](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
After enabling the diagnostic output, rebuild the Xamarin.Android
binding project and examine the build log to locate clues about what
the cause of problem is.

It can also prove helpful to decompile the Android library and examine
the types and methods that Xamarin.Android is trying to bind. This is
covered in more detail later on in this guide.

## Decompiling an Android Library

Inspecting the classes and methods of the Java classes can provide
valuable information that will assist in binding a library.
[JD-GUI](http://jd.benow.ca/) is a graphical utility that can display
Java source code from the **CLASS** files contained in a JAR. It can be
run as a stand alone application or as a plug-in for IntelliJ or
Eclipse.

To decompile an Android library open the **.JAR** file with the Java
decompiler. If the library is an **.AAR** file, it is necessary to
extract the file **classes.jar** from the archive file. The following
is a sample screenshot of using JD-GUI to analyze the
[Picasso](https://square.github.io/picasso/) JAR:

![Using the Java Decompiler to analyze picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Once you have decompiled the Android library, examine the source code. Generally speaking, look for :

- **Classes that have characteristics of obfuscation** &ndash;
  Characteristics of obfuscated classes include:

  - The class name includes a **$**, i.e. **a$.class**
  - The class name is entirely compromised of lower case characters, i.e. **a.class**      

- **`import` statements for unreferenced libraries** &ndash; Identify
  the unreferenced library and add those dependencies to the
  Xamarin.Android binding project with a **Build Action** of
  **ReferenceJar** or **EmbedddedReferenceJar**.

> [!NOTE]
> Decompiling a Java library may be prohibited or
> subject to legal restrictions based on local laws or the license under
> which the Java library was published. If necessary, enlist the services
> of a legal professional before attempting to decompile a Java library
> and inspect the source code.

## Inspect API.XML

As a part of building a binding project, Xamarin.Android will generate
an XML file name **obj/Debug/api.xml**:

![Generated api.xml under obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

This file provides a list of all the Java APIs that Xamarin.Android is
trying bind. The contents of this file can help identify any missing
types or methods, duplicate binding. Although inspection of this file
is tedious and time consuming, it can provide for clues on what might
be causing any binding problems. For example, **api.xml** might reveal
that a property is returning an inappropriate type, or that there are
two types that share the same managed name.

## Known Issues

This section will list some of the common error messages or symptoms
that my occur when trying to bind an Android library.

### Problem: Java Version Mismatch

Sometimes types will not be generated or unexpected crashes may occur
because you are using either a newer or older version of Java compared
to what the library was compiled with. Recompile the Android library
with the same version of the JDK that your Xamarin.Android project
is using.

### Problem: At least one Java library is required

You receive the error "at least one Java library is required," even
though a .JAR has been added.

#### Possible Causes:

Make sure the build action is set to `EmbeddedJar`. Since there are
multiple build actions for .JAR files (such as `InputJar`,
`EmbeddedJar`, `ReferenceJar` and `EmbeddedReferenceJar`), the binding
generator cannot automatically guess which one to use by default. For
more information about build actions, see
[Build Actions](~/android/platform/binding-java-library/index.md).

### Problem: Binding tools cannot load the .JAR library

The binding library generator fails to load the .JAR library.

#### Possible Causes

Some .JAR libraries that use code obfuscation (via tools such as
Proguard) cannot be loaded by the Java tools. Since our tool makes use
of Java reflection and the ASM byte code engineering library, those
dependent tools may reject the obfuscated libraries while Android
runtime tools may pass. The workaround for this is to hand-bind these
libraries instead of using the binding generator.

### Problem: Missing C# types in generated output.

The binding **.dll** builds but misses some Java types, or the
generated C# source does not build due to an error stating there are
missing types.

#### Possible Causes:

This error may occur due to several reasons as listed below:

- The library being bound may reference a second Java library. If
  the public API for the bound library uses types from the second
  library, you must reference a managed binding for the second
  library as well.

- It is possible that a library was injected due to Java reflection,
  similar to the reason for the library load error above, causing the
  unexpected loading of metadata. Xamarin.Android's tooling cannot
  currently resolve this situation. In such a case, the library must
  be manually bound.

- There was a bug in .NET 4.0 runtime that failed to load assemblies
  when it should have. This issue has been fixed in the .NET 4.5
  runtime.

- Java allows deriving a public class from non-public class, but this
  is unsupported in .NET. Since the binding generator does not
  generate bindings for non-public classes, derived classes such as
  these cannot be generated correctly. To fix this, either remove the
  metadata entry for those derived classes using the remove-node in
  **Metadata.xml**, or fix the metadata that is making the non-public
  class public. Although the latter solution will create the binding
  so that the C# source will build, the non-public class should not
  be used.

  For example:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Tools that obfuscate Java libraries may interfere with the
  Xamarin.Android Binding Generator and its ability to generate C#
  wrapper classes. The following snippet shows how to update
  **Metadata.xml** to unobfuscate a class name:

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### Problem: Generated C# source does not build due to parameter type mismatch

The generated C# source does not build. Overridden method's parameter
types do not match.

#### Possible Causes:

Xamarin.Android includes a variety of Java fields that are mapped to
enums in the C# bindings. These can cause type incompatibilities in the
generated bindings. To resolve this, the method signatures created from
the binding generator need to be modified to use the enums. For more
information, please see
[Correcting Enums](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### Problem: NoClassDefFoundError in packaging

`java.lang.NoClassDefFoundError` is thrown in the packaging step.

#### Possible Causes:

The most likely reason for this error is that a mandatory Java library
needs to be added to the application project (**.csproj**). .JAR files
are not automatically resolved. A Java library binding is not always
generated against a user assembly that does not exist in the target
device or emulator (such as Google Maps **maps.jar**). This is not the
case for Android Library project support, as the library .JAR is
embedded in the library dll. For example:
[Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### Problem: Duplicate custom EventArgs types

Build fails due to duplicate custom EventArgs types. An error like this
occurs:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### Possible Causes:

This is because there is some conflict between event types that come
from more than one interface "listener" type that shares methods having
identical names. For example, if there are two Java interfaces as seen
in the example below, the generator creates `DismissScreenEventArgs`
for both `MediationBannerListener` and `MediationInterstitialListener`,
resulting in the error.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

This is by design so that lengthy names on event argument types are
avoided. To avoid these conflicts, some metadata transformation is
required. Edit **Transforms\Metadata.xml** and add an `argsType` attribute on either of the interfaces (or on the
interface method):

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### Problem: Class does not implement interface method

An error message is produced indicating that a generated class does not
implement a method that is required for an interface which the
generated class implements. However, looking at the generated code, you
can see that the method is implemented.

Here is an example of the error:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### Possible Causes:

This is a problem that occurs with binding Java methods with covariant
return types. In this example, the method
`Oauth.Signpost.Http.IHttpRequest.UnWrap()` needs to return
`Java.Lang.Object`. However, the method
`Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` has a
return type of `HttpURLConnection`. There are two ways to fix this
issue:

- Add a partial class declaration for
  `HttpURLConnectionRequestAdapter` and explicitly implement
  `IHttpRequest.Unwrap()`:

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- Remove the covariance from the generated C# code. This involves
  adding the following transform to **Transforms\Metadata.xml** which
  will cause the generated C# code to have a return type of
  `Java.Lang.Object`:

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### Problem: Name Collisions on Inner Classes / Properties

Conflicting visibility on inherited objects.

In Java, it's not required that a derived class have the same
visibility as its parent. Java will just fix that for you. In C#, that
has to be explicit, so you need to make sure all classes in the
hierarchy have the appropriate visibility. The following example shows
how to change a Java package name from `com.evernote.android.job` to
`Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### Problem: A **.so** Library Required by the Binding is Not Loading

Some binding projects may also depend on functionality in a **.so**
library. It is possible that Xamarin.Android will not automatically
load the **.so** library. When the wrapped Java code executes,
Xamarin.Android will fail to make the JNI call and the error message
_java.lang.UnsatisfiedLinkError: Native method not found:_ will appear
in the logcat out for the application.

The fix for this is to manually load the **.so** library with a call to
`Java.Lang.JavaSystem.LoadLibrary`. For example assuming that a
Xamarin.Android project has shared library **libpocketsphinx_jni.so**
included in the binding project with a build action of
**EmbeddedNativeLibrary**, the following snippet (executed
before using the shared library) will load the **.so** library:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## Summary

In this article, we listed common troubleshooting issues associated
with Java Bindings and explained how to resolve them.

## Related Links

- [Library Projects](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Working with JNI](~/android/platform/java-integration/working-with-jni.md)
- [Enable Diagnostic Output](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin for Android Developers](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
