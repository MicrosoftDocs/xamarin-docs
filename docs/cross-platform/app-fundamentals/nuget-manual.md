---
title: "Manually Creating NuGet Packages for Xamarin"
description: "This document contains tips to help build NuGet packages that target the Xamarin platform. It describes NuGet package Xamarin profiles, PCL NuGets with platform dependencies, and links to various open-source samples."
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
---

# Manually Creating NuGet Packages for Xamarin

_This page contains some tips to help build NuGet packages that target the Xamarin platform._

> [!NOTE]
> Xamarin Studio 6.2 (and Visual Studio for Mac) includes the ability to
> _automatically_ generate NuGet packages from PCL, .NET Standard, or
> Shared Projects. Refer to the 
> [Multiplatform Libraries for Code Sharing](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
> guide for more details.

## NuGet Package Xamarin Profiles

The NuGet website's [Supporting Multiple .NET Framework Versions and Profiles](https://docs.nuget.org/create/enforced-package-conventions)
discusses how to support different Microsoft frameworks and profiles
but does not include the target framework names used by Xamarin.

The main Xamarin target frameworks in use today are:

- **MonoAndroid** - Xamarin.Android
- **Xamarin.iOS** - Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (supports 64-bit)
- **Xamarin.Mac** - Xamarin.Mac's mobile profile, which is equivalent
  to the Xamarin.iOS and Xamarin.Android API surface.

There is also a target for the older iOS [Classic API](~/cross-platform/macios/unified/index.md):

- **MonoTouch** - iOS Classic API

A **.nuspec** file that targeted all these would look something like:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

The above ignores any portable class libraries.

Most **.nuspec** files specify the version number of the target framework but it is
optional if your assembly works with all versions of that target framework. So
if your target was **lib\MonoAndroid** this would mean it works with any version
of Xamarin.Android.

You can specify the version with a set of numbers without a decimal point or
you can specify it using decimal points. Without the decimal point NuGet will
just take each number and turn it into a version by inserting a '.' between
each digit.

In the above "MonoAndroid10" means "Android 1.0". This just means the
project's [target framework](~/android/app-fundamentals/android-api-levels.md)
needs to be MonoAndroid version 1.0 or higher. The
version is specified in the `<TargetFrameworkVersion>` element in the project
file.

To clarify:

- **MonoAndroid403** matches Android 4.0.3 and newer (ie API level 15)
- **Xamarin.iOS10** matches Xamarin.iOS 1.0 and newer
- **Xamarin.iOS1.0** also matches Xamarin.iOS 1.0 and newer

## PCL NuGets with Platform Dependencies

PCL Profiles are limited in what .NET framework APIs they can access,
and they certainly can't access platform-specific code. These 3rd-party links
discuss different approaches for creating NuGet packages that use PCL
and native APIs to provide compatibility for Xamarin and other platforms:

- [How to Make Portable Class Libraries Work for You](https://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [The Bait and Switch PCL Trick](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Creating a NuGet PCL that works with Xamarin.iOS](https://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

This external [list of PCL Profiles with their NuGet target name](https://portablelibraryprofiles.stephencleary.com)
is also a useful reference.

## Examples

Some open-source examples that you can refer to:

- [**ModernHttpClient**](https://www.nuget.org/packages/modernhttpclient/) – Write your app using System.Net.Http, but drop this library in and it will go drastically faster (view [source](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat**](https://www.nuget.org/packages/Splat/) – A library to make things cross-platform that should be (view [source](https://github.com/paulcbetts/Splat)).
- [**NGraphics**](https://www.nuget.org/packages/NGraphics/) - A cross platform library for rendering vector graphics on .NET (view [source](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## Related Links

- [Nugetizer-3000 Automated NuGet Creation](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [Including a NuGet in your Project](/visualstudio/mac/nuget-walkthrough)