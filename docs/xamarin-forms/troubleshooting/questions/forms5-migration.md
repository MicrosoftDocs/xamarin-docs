---
title: "How do I migrate my app to Xamarin.Forms 5.0?"
description: "How to migrate your app to Xamarin.Forms 5.0, with focus on Android on UWP."
ms.assetid: AD04FEE9-B8F5-4CA5-AB31-EF1225867E4B
ms.prod: xamarin
ms.technology: xamarin-forms
ms.topic: troubleshooting
author: davidbritch
ms.author: dabritch
ms.date: 10/20/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# How do I migrate my app to Xamarin.Forms 5.0?

Xamarin.Forms 5.0 includes the following breaking changes:

- `Expander` has moved to the Xamarin Community Toolkit. For more information, see [Features moved from Xamarin.Forms](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms).
- `MediaElement` has moved to the Xamarin Community Toolkit. For more information, see [Features moved from Xamarin.Forms](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms).
- DataPages, and associated projects, have been removed from Xamarin.Forms.
- `MasterDetailPage` has been renamed to `FlyoutPage`. Similarly, the `MasterBehavior` enumeration has been renamed to `FlyoutLayoutBehavior`.
- References to `UIWebView` have been removed from Xamarin.Forms on iOS.
- Support for Visual Studio 2017 has been removed.
- XFCorePostProcessor.Tasks has been removed. This project injected IL to maintain Xamarin.Forms 2.5 compatibility.

In addition, Android and UWP projects built with Xamarin.Forms 5.0 will require updating.

## Android

Android projects built with Xamarin.Forms 5.0 require that you've installed the AndroidX (Android 10.0) platform to your development environment. This can be accomplished with the Android SDK manager. For more information about AndroidX, see [AndroidX migration in Xamarin.Forms](~/xamarin-forms/platform/android/androidx-migration.md).

Android projects will then require several updates to build correctly.

### Minimum TargetFrameworkVersion

Xamarin.Forms 5.0 requires a minimum target framework version of 10.0 (AndroidX) for Android projects. The target framework version can be set in Visual Studio, or in the Android .csproj file:

```xml
<TargetFrameworkVersion>v10.0</TargetFrameworkVersion>
```

A build error will be produced if this minimum requirement isn't met:

```
error XF005: The $(TargetFrameworkVersion) for MyProject.Android (v9.0) is less than the minimum required $(TargetFrameworkVersion) for Xamarin.Forms (10.0). You need to increase the $(TargetFrameworkVersion) for MyProject.Android.
```

### Minimum TargetSDKVersion

AndroidX requires that your Android manifest sets the `targetSdkVersion` to 29+:

```xml
<uses-sdk android:minSdkVersion="21" android:targetSdkVersion="29" />
```

Failure to do this will cause the `targetSdkVersion` to be set to the `minSdkVersion`. In addition, in some circumstances a `Android.Views.InflateException` will be produced if the `targetSdkVersion` isn't correctly set:

```
Android.View.InflateException has been thrown.

Binary XML file line #1 in com.companyname.myproject:layout/toolbar: Binary XML file line #1 in com.companyname.myproject:/layout/toolbar: Error inflating class android.support.v7.widget.Toolbar
```

> [!NOTE]
> In Visual Studio 2019, the Android manifest will automatically be updated to specify a `targetSdkVersion` of API 29 when the target framework version is set to v10.0.

### Automatic migration to AndroidX

If your Android project references any Android support libraries, either as direct dependencies or transitive dependencies, these support library dependencies and bindings are automatically swapped with AndroidX dependencies during the build process. For more information about automatic AndroidX migration, see [Automatic migration in Xamarin.Forms](~/xamarin-forms/platform/android/androidx-migration.md#automatic-migration-in-xamarinforms).

### Manual migration to AndroidX

If your Android project doesn't have direct or transitive dependencies on Android support libraries, but still attempts to consume support library types through code, you will have to manually migrate your app to AndroidX. This can be accomplished by using AndroidX types, and by removing any AXML files that you haven't customized.

> [!TIP]
> Manual migration to AndroidX will result in the fastest build process for your app.

#### Use AndroidX types

AndroidX replaces the Android support libraries, and so any references to Android support library types must be replaced with references to AndroidX types.

This can be accomplished by updating your `using` statements to use `AndroidX` namespaces, rather than `Android.Support` namespaces. The following table lists some of the common namespace changes when moving from the Android support libraries to AndroidX:

| Android support library namespace | AndroidX namespace |
| --- | --- |
| `Android.Support.V4.App` | `AndroidX.Core.App` |
| `Android.Support.V4.Content` | `AndroidX.Core.Content` |
| `Android.Support.V4.App` | `AndroidX.Fragment.App` |
| `Android.Support.V7.App` | `AndroidX.AppCompat.App` |
| `Android.Support.V7.Widget` | `AndroidX.AppCompat.Widget` |

For a complete list of class mappings from support libraries to AndroidX, see [AndroidX class mappings](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-class-mapping.csv) on github.com. For a complete list of assembly mappings from support libraries to AndroidX, see [AndroidX assemblies](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-assemblies.csv) on github.com

#### Remove AXML files

You should delete any AXML files from your Android project, provided that it doesn't use customized AXML files. After deletion, the following lines should be removed from your `MainActivity` class:

```csharp
TabLayoutResource = Resource.Layout.Tabbar;
ToolbarResource = Resource.Layout.Toolbar;
```

> [!IMPORTANT]
> Android projects with customized AXML files should be updated so that these files use AndroidX types.

## UWP

Xamarin.Forms 5.0 recommends a target platform version of >= 10.0.18362.0 for UWP projects. The target platform version can be set in Visual Studio, or in the UWP .csproj file:

```xml
<TargetPlatformVersion Condition=" '$(TargetPlatformVersion)' == '' ">10.0.18362.0</TargetPlatformVersion>
```

A build warning will be produced if your UWP project uses a lower target platform version.

## Related links

- [Features moved from Xamarin.Forms](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms)
- [AndroidX migration in Xamarin.Forms](~/xamarin-forms/platform/android/androidx-migration.md)
- [AndroidX class mappings](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-class-mapping.csv)
- [AndroidX assemblies](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-assemblies.csv)
