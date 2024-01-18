---
title: "Linking on Android"
ms.service: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/30/2018
description: Learn how to use Xamarin.Android applications to use a linker to reduce the size of an application with supporting information.
---

# Linking on Android

Xamarin.Android applications use a *linker* to reduce the
size of the application. The linker employs static analysis of your application
to determine which assemblies are actually used, which types are actually used,
and which members are actually used. The linker then behaves like a *garbage collector*, continually looking for the assemblies, types, and members that
are referenced until the entire closure of referenced assemblies, types, and
members is found. Then everything outside of this closure is *discarded*.

For example, the [Hello, Android](/samples/xamarin/monodroid-samples/hellom4a) sample:

|Configuration|1.2.0 Size|4.0.1 Size|
|---|---|---|
|Release without Linking:|14.0 MB|16.0 MB|
|Release with Linking:|4.2 MB|2.9 MB|

Linking results in a package that is 30% the size of the original
(unlinked) package in 1.2.0, and 18% of the unlinked package in 4.0.1.

## Control

Linking is based on *static analysis*. Consequently, anything that
depends upon the runtime environment won't be detected:

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

### Linker Behavior

The primary mechanism for controlling the linker is the **Linker Behavior** (*Linking* in Visual Studio)
drop-down within the **Project Options** dialog box. There are three options:

1. **Don't Link** (*None* in Visual Studio)
1. **Link SDK Assemblies** (*Sdk Assemblies Only*)
1. **Link All Assemblies** (*Sdk and User Assemblies*)

The **Don't Link** option turns off the linker; the above "Release
without Linking" application size example used this behavior. This is useful for
troubleshooting runtime failures, to see if the linker is responsible. This setting
is not usually recommended for production builds.

The **Link SDK Assemblies** option only links
[assemblies that come with Xamarin.Android](~/cross-platform/internals/available-assemblies.md).
All other assemblies (such as your code) are not linked.

The **Link All Assemblies** option links all assemblies, which means your
code may also be removed if there are no static references.

The above example will work with the *Don't Link* and *Link SDK Assemblies* options,
and will fail with the *Link All Assemblies*
behavior, generating the following error:

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

### Preserving Code

The linker will sometimes remove code that you want to
preserve. For example:

- You might have code that you call dynamically via
    `System.Reflection.MemberInfo.Invoke`.

- If you instantiate types dynamically, you may want to preserve the
    default constructor of your types.

- If you use XML serialization, you may want to preserve the
    properties of your types.

In these cases, you can use the
[Android.Runtime.Preserve](xref:Android.Runtime.PreserveAttribute)
attribute. Every member that is not statically linked by the
application is subject to be removed, so this attribute can be used to
mark members that are not statically referenced but are still needed by
your application. You can apply this attribute to every member of a
type, or to the type itself.

In the following example, this attribute is used to preserve
the constructor of the `Example` class:

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

If you want to preserve the entire type, you can use the following
attribute syntax:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

For example, in the following code fragment the entire `Example` class
is preserved for XML serialization:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

Sometimes you want to preserve certain members, but only if the
containing type was preserved. In those cases, use the following
attribute syntax:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

If you do not want to take a dependency on the Xamarin libraries
&ndash; for example, you are building a cross platform portable class
library (PCL) &ndash; you can still use the `Android.Runtime.Preserve`
attribute. To do this, declare a `PreserveAttribute` class within the
`Android.Runtime` namespace like this:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

In the above examples, the `Preserve` attribute is declared in the
`Android.Runtime` namespace; however, you can use the `Preserve`
attribute in any namespace because the linker looks up this attribute
by type name.

### falseflag

If the [Preserve] attribute can't be used, it is often useful to provide a
block of code so that the linker believes that the type is used, while
preventing the block of code from being executed at runtime. To make use of this
technique, we could do:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```

### linkskip

It is possible to specify that a set of user-provided assemblies should not
be linked at all, while allowing other user assemblies to be skipped with the *Link SDK Assemblies* behavior by using the [AndroidLinkSkip MSBuild property](~/android/deploy-test/building-apps/build-process.md):

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

### LinkDescription

The [`@(LinkDescription)`](~/android/deploy-test/building-apps/build-process.md)
**Build action** may be used on files which can contain a
[Custom linker configuration file](~/cross-platform/deploy-test/linker.md).
file. Custom linker configuration files may be required to preserve
`internal` or `private` members that need to be preserved.

### Custom Attributes

When an assembly is linked, the following custom attribute types will be
removed from all members:

- System.ObsoleteAttribute
- System.MonoDocumentationNoteAttribute
- System.MonoExtensionAttribute
- System.MonoInternalNoteAttribute
- System.MonoLimitationAttribute
- System.MonoNotSupportedAttribute
- System.MonoTODOAttribute
- System.Xml.MonoFIXAttribute

When an assembly is linked, the following custom attribute types will be
removed from all members in Release builds:

- System.Diagnostics.DebuggableAttribute
- System.Diagnostics.DebuggerBrowsableAttribute
- System.Diagnostics.DebuggerDisplayAttribute
- System.Diagnostics.DebuggerHiddenAttribute
- System.Diagnostics.DebuggerNonUserCodeAttribute
- System.Diagnostics.DebuggerStepperBoundaryAttribute
- System.Diagnostics.DebuggerStepThroughAttribute
- System.Diagnostics.DebuggerTypeProxyAttribute
- System.Diagnostics.DebuggerVisualizerAttribute

## Related Links

- [Custom Linker Configuration](~/cross-platform/deploy-test/linker.md)
- [Linking on iOS](~/ios/deploy-test/linker.md)
