---
title: "Smarter Xamarin Android Support v4 / v13 NuGet Packages"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
---

# Smarter Xamarin Android Support v4 / v13 NuGet Packages

## About the Android Support Libraries

Google has created support libraries to make new features available to older versions of Android. In general, Support Libraries are given a version number in their name, which is the lowest Android API Level they are compatible with (eg: Support-v4 can only be used on API Level 4 and higher. More info in this [Stack Overflow discussion](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Two of the support libraries: `Support-v4` and `Support-v13` can not be used together in the same app, that is, they are mutually exclusive. This is because `Support-v13` actually contains all of the types and implementation of `Support-v4`. If you try and reference both in the same project you will encounter duplicate type errors.

## Problems with Referencing

Since `Support-v4` has become so popular, a lot of 3rd party libraries now depend on it. They could have chosen to depend on Support-v13 instead, but it's more common to depend on _v4_ since that gives any apps using these 3rd party libraries the option of supporting API levels all the way down to 4.

If a Xamarin 3rd party library references the `Xamarin.Android.Support.v4.dll` binding to `Support-v4`, any app that uses this library must also reference `Xamarin.Android.Support.v4.dll`. This becomes a problem when the same app also wants to use some of the functionality from the `Xamarin.Android.Support.v13.dll` binding to `Support-v13`. If you reference both bindings, you will encounter duplicate type errors.

## Type-Forwarded v4 Binding Assembly

To get around this problem, we have created a special `Xamarin.Android.Support.v4.dll` assembly which has no implementation, but simply `[assembly: TypeForwardedTo (..)]` attributes which forward all of the `Support-v4` types to the implementation within the `Xamarin.Android.Support.v13.dll` assembly.

This means a developer can reference this _type-forwarded_ assembly in their app which will satisfy the reference to `Xamarin.Android.Support.v4.dll` by any 3rd party libraries, while still allowing `Xamarin.Android.Support.v13.dll` to be used in the app.

## NuGet Assistance

While a developer could manually add the correct references necessary, we are able to use NuGet to help choose the right assembly (either the normal _v4_ binding or the type-forwarded _v4_ assembly) when the NuGet package is installed.

So, the `Xamarin.Android.Support.v4` NuGet package now contains the following logic:

If your app is targeting API Level 13 (Gingerbread 3.2) or higher:

*   `Xamarin.Android.Support.v13` NuGet will automatically be added as a dependency
*   The _type-forwarded_ `Xamarin.Android.Support.v4.dll` will be referenced in the project

If your app is targeting anything lower than API Level 13, you will get the normal `Xamarin.Android.Support.v4.dll` binding referenced in your project.

## Do I have to use Support-v13?

If your app is targeting API Level 13 or higher and you choose to use the `Xamarin Android Support-v4` NuGet package, then the `Xamarin Android Support v13` NuGet package is a required dependency.

We feel the very minor increase in app size (the two .jar files differ by 17kb) is well worth the compatibility and fewer headaches it results in.

If you are adamant about using `Support-v4` in an app that targets API Level 13 or higher, you can always manually download the `.nupkg`, extract it, and reference the assembly.
