---
title: "Debuggable Attribute"
description: Android supports the Java Debug Wire Protocol. Learn how this technology makes debugging possible by allowing tools such as ADB to communicate with a JVM.
ms.service: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 12/13/2019
---

# Debuggable Attribute

To make debugging possible, Android supports the Java Debug Wire Protocol (JDWP). This is a technology that allows tools such as ADB to communicate with a JVM. While JDWP is important during development, it should be disabled prior to the application being published.

JDWP can be configured by the value of the `android:debuggable` attribute in an Android application. Choose _one_ of the following three ways to set this attribute in Xamarin.Android:

## AndroidManifest.xml

Create or open `AndroidManifext.xml` file, and set the `android:debuggable` attribute there. Take extra care not to ship your release build with debugging enabled.

```xml
 	<application android:label="@string/app_name"
               android:debuggable="true"
               android:icon="@mipmap/appicon">
    ...
	</application>
```

## Add an Application class attribute

If your Xamarin.Android app has a class with an `[Application]` attribute, update the attribute to `[Application(Debuggable = true)]`. Set it to `false` to disable.

## Add an assembly attribute

If your Xamarin.Android app does NOT already have an `[Application]` class attribute, add an assembly-level attribute `[assembly: Application(Debuggable=true)]` in a c# file such as, `Properties\AssemblyInfo.cs`. Set it to `false` to disable.

## Summary

If both the `AndroidManifest.xml` and the `ApplicationAttribute` are present, the contents of `AndroidManifest.xml` take priority over what is specified by the `ApplicationAttribute`.

If you add both a class attribute _and_ an assembly attribute, there will be a compiler error:

```error
"Error The "GenerateJavaStubs" task failed unexpectedly.
System.InvalidOperationException: Application cannot have both a type with an [Application] attribute and an [assembly:Application] attribute."
```

By default – if neither the `AndroidManifest.xml` nor the `ApplicationAttribute` is present – the value of the `android:debuggable` attribute depends on whether or not debug symbols are generated. If debug symbols are present, then Xamarin.Android will set the `android:debuggable` attribute to `true` for you.

> [!WARNING]
> The value of the `android:debuggable` attribute does NOT necessarily depend on the build configuration. It is possible for release builds to have the `android:debuggable` attribute set to true. If you use an attribute to set this value, you can choose to wrap the attribute in a compiler directive:
>
> ```csharp
> #if DEBUG
> [Application(Debuggable = true)]
> #else
> [Application(Debuggable = false)]
> #endif
> ```

## Related Links

- [Debuggable apps in the Android market](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
