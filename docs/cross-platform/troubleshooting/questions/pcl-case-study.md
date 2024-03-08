---
title: "Resolve problems related to System.Diagnostics.Tracing and TPL Dataflow"
description: "PCL case study: How can I resolve problems related to System.Diagnostics.Tracing for the Microsoft TPL Dataflow NuGet package?"
ms.service: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
no-loc: [Objective-C]
author: davidortinau
ms.author: daortin
---
# PCL case study: How can I resolve problems related to System.Diagnostics.Tracing for the Microsoft TPL Dataflow NuGet package?

> [!IMPORTANT]
> This particular example of `System.Diagnostic.Tracing` no longer produces any errors by default in the latest versions of Xamarin. While the workaround suggested will still work, note that some of the bugs mentioned in the "layers of errors" section have been fixed.
> Furthermore, you should note that .NET Standard is now the preferred way of implementing cross-platform .NET APIs.

## Summary

Xamarin.iOS and Xamarin.Android do not implement 100% of every PCL profile that they allow as references. For practical convenience in Visual Studio for Mac, Visual Studio, and the NuGet package manager, Xamarin projects allow the use of several profiles that only have _incomplete_ implementations. For example, neither Xamarin.iOS nor Xamarin.Android currently includes a complete implementation of the types in the "System.Diagnostics.Tracing" PCL namespace. This limitation leads to three layers of errors when trying to use the default `portable-net45+win8+wpa81` version of the Microsoft TPL Dataflow NuGet package.

## Workaround: Switch the app project to reference the `portable-net45+win8+wp8+wpa81` version of the TPL Dataflow library

(This avoids all three layers of errors and works for all recent versions of Xamarin.)

1. Open the application project **.csproj** file in a text editor.

2. Find the line that looks similar to this:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Change `portable-net45+win8+wpa81` to `portable-net45+win8+wp8+wpa81` (`+wp8` is added):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### Explanation

The `portable-net45+win8+wp8+wpa81` version of the library does not reference **System.Diagnostics.Tracing.dll** _at all_, so it completely avoids all three layers of problems.

### Limitations

- The `portable-net45+win8+wp8+wpa81` version of the library might not include 100% of the functionality of the `portable-net45+win8+wpa81` version.

- The NuGet package manager installs the `portable-net45+win8+wpa81` version of the PCL NuGet package by default, so you must adjust the reference by hand.

## Details about the three layers of errors

1. The **System.Diagnostics.Tracing.dll** facade assembly is currently absent from all Mac versions of Xamarin.Android (non-public Bug 34888) and absent from all Xamarin.iOS versions lower than 9.0 (or lower than XamarinVS 3.11.1443 on Windows) (fixed in Bug 32388). This problem will cause one of the following errors depending on deployment target and linker settings:

    - Xamarin.Android.Common.targets: Error: Exception while loading assemblies: System.IO.FileNotFoundException: Could not load assembly 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'. Perhaps it doesn't exist in the Mono for Android profile?

    - Could not load file or assembly 'System.Diagnostics.Tracing' or one of its dependencies. The system cannot find the file specified. (System.IO.FileNotFoundException)

    - MTOUCH: error MT3001: Could not AOT the assembly '/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll'

    - MTOUCH: error MT2002: Failed to resolve assembly: 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. The current [Mono implementation of the types in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) is missing some method overloads. This problem will cause one of the following linker errors when building a Xamarin app:

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: error : Error executing task LinkAssemblies: error XA2006: Reference to metadata item 'System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])' (defined in 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a') from 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' could not be resolved.

    - MTOUCH: error MT2002: Failed to resolve "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" reference from "System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"

3. The current [Mono implementation of the types in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) is also currently an _empty_ "dummy" implementation (Bug 34890). Any attempt to use these methods at run time might therefore produce unexpected results. For the _particular_ case of the Microsoft TPL Dataflow Library, it seems the calls to `WriteEvent(System.Int32,System.Object[])` are not essential for most of the library's behavior, so the fix for "layer 2" (Bug 27337, adding empty implementations) will likely be sufficient for most Microsoft TPL Dataflow use cases.

## Questions & Answers

### I was able to leave linking enabled with the `portable-net45+win8+wpa81` version of the library on older versions of Xamarin.iOS or on Xamarin.Android. How did that work?

#### Answer

It is _possible_ to get the build to complete "successfully" (with linking enabled) in older versions of Xamarin.iOS or in Xamarin.Android on Mac if you include a reference to the `System.Diagnostics.Tracing.dll` _reference assembly_ \[1\] rather than the _facade assembly_ \[2], but unfortunately this is not a "correct" workaround. Reference assemblies are only meant to be used when building _portable libraries_, not platform-specific code like apps. Attempting to _run_ the code contained in reference assemblies (rather than just build against it) is likely to produce unexpected results. The correct fix will be for the Mono team to add the missing `WriteEvent(System.Int32,System.Object[])` overload to the [`EventSource`](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) type (Bug 27337). For now the best option is to switch to the `portable-net45+win8+wp8+wpa81` version of the Microsoft TPL Dataflow library as discussed in the Workaround section above.

(For anyone who might be reading this article after seeing a related older, briefer answer from StackOverflow (<https://stackoverflow.com/a/23591322/2561894>), note that the distinction between reference assemblies and facade assembly was _not_ mentioned there.)

**\[1\] "Reference assembly" locations**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "Facade assembly" locations**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

### Will it help if I manually add a reference to the "System.Diagnostics.Tracing" facade assembly?

_In particular can I solve the problem using these 2 steps?_

1. _Copy the `System.Diagnostics.Tracing.dll` facade assembly into the application project folder from one of the following locations:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Add a reference to the facade assembly in the Xamarin.iOS or Xamarin.Android application project._

#### Answer

No, this will not help.

- For Xamarin.iOS 9.0 or any recent version of Xamarin.Android on Windows, this workaround is strictly redundant, and might cause compile errors similar to "An assembly `System.Diagnostics.Tracing' with the same identity has already been imported.".

- For Xamarin.iOS 8.10 or lower or for Xamarin.Android on Mac, this workaround will help but _only_ for the "layer 1" missing assembly problem. It will _not_ solve the "layer 2" linker errors, so it is not a complete solution.

### Can I use the [System.Diagnostics.Tracing NuGet package](https://www.nuget.org/packages/System.Diagnostics.Tracing/) to solve the problem?

#### Answer

No, the NuGet 3.0 "System.Diagnostics.Tracing" package only includes platform-specific implementations for "DNXCore50" and "netcore50". It explicitly _omits_ implementations for Xamarin.Android ("MonoAndroid") and Xamarin.iOS ("MonoTouch" and "xamarinios"). This means that installing the package will have _no effect_ for Xamarin.Android and Xamarin.iOS projects. The NuGet package assumes that both of those platforms provide their _own_ implementation of the types. This assumption is "correct" in the sense that Mono does have _an_ implementation of the namespace, but as discussed under points \#2 and \#3 of "Details about the three layers of errors" above, the implementation is currently incomplete. So the  proper fix will be for the Mono team to resolve Bug 27337 and Bug 34890.

## Next Steps

For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed.
