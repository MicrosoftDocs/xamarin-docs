---
title: "AndroidX Migration in Xamarin.Forms"
description: "This article explains why AndroidX exists and how to migrate to AndroidX in your Xamarin.Forms app."
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/13/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# AndroidX migration in Xamarin.Forms

AndroidX replaces the Android Support Library. This article explains why AndroidX exists, how it impacts Xamarin.Forms, and how to migrate your application to use the AndroidX libraries.

> [!IMPORTANT]
> If you are migrating an app to Xamarin.Forms 5.0, see [How do I migrate my app to Xamarin.Forms 5.0?](~/xamarin-forms/troubleshooting/questions/forms5-migration.md).

## History of AndroidX

The Android Support Library was created to provide newer features on older versions of Android. It is a compatibility layer that allows developers to use functionality that may not exist on all versions of the Android operating system and have graceful fallbacks for older versions. The Support Library also includes convenience and helper classes, debugging and utility tools, and sophisticated classes that depend on other Support Library classes to function.

While the Support Library was originally a single binary, it has grown and evolved into a suite of libraries, which are almost essential for modern app development. These are some commonly used features from the Support Library:

- The `Fragment` support class.
- The `RecyclerView`, used for managing long lists.
- Multidex support for apps with over 65,536 methods.
- The `ActivityCompat` class.

AndroidX is a replacement for the Support Library, which is no longer maintained - all new library development will occur in the AndroidX library. AndroidX is a redesigned library that uses semantic versioning, clearer package names, and better support for common application architecture patterns. AndroidX version 1.0.0 is the binary equivalent to Support Library version 28.0.0. For a complete list of class mappings from Support Library to AndroidX, see [Support Library class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) on developer.android.com.

Google created a migration process called the Jetifier with AndroidX. The Jetifier inspects the jar bytecode during the build process and remaps Support Library references, both in app code and in dependencies, to their AndroidX equivalent.

In a Xamarin.Forms app, just as in an Android Java app, the jar dependencies must be migrated to AndroidX. However, the Xamarin bindings must also be migrated to point to the correct, underlying jar files. Xamarin.Forms added support for automatic AndroidX migration in version 4.5.

For more information about AndroidX, see [AndroidX overview](https://developer.android.com/jetpack/androidx) on developer.android.com.

## Automatic migration in Xamarin.Forms

To automatically migrate to AndroidX, a Xamarin.Forms Android platform project must:

- Target Android API version 29 or greater.
- Use Xamarin.Forms version 4.5 or greater.
- Have direct or transitive dependencies on Android support libraries.

Once you have confirmed these settings in your project, build the Android app in Visual Studio 2019. During the build process, the Intermediate Language (IL) is inspected and Support Library dependencies and bindings are swapped with AndroidX dependencies. If your application has all of the AndroidX dependencies required to build, you will notice no differences in the build process.

> [!IMPORTANT]
> Manual migration to AndroidX will result in the fastest build process for your app, and is the recommended approach for AndroidX migration. This involves replacing support library dependencies with AndroidX dependencies, and updating your code to consume AndroidX types. For more information, see [Use AndroidX types](~/xamarin-forms/troubleshooting/questions/forms5-migration.md#use-androidx-types).

If AndroidX dependencies are detected that are not part of the project, a build error is reported that indicates which AndroidX packages are missing. An example build error is shown below:

```
Could not find 37 AndroidX assemblies, make sure to install the following NuGet packages:
- Xamarin.AndroidX.Lifecycle.LiveData
- Xamarin.AndroidX.Browser
- Xamarin.Google.Android.Material
- Xamarin.AndroidX.Legacy.Supportv4
You can also copy and paste the following snippit into your .csproj file:
 <PackageReference Include="Xamarin.AndroidX.Lifecycle.LiveData" Version="2.1.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Browser" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.Google.Android.Material" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Legacy.Support.V4" Version="1.0.0-rc1" />
```

The missing NuGet packages can either be installed via the NuGet Package Manager in Visual Studio, or installed by editing your Android .csproj file to include the `PackageReference` XML items listed in the error.

Once the missing packages are resolved, rebuilding the project loads the missing packages and your project is compiled using AndroidX dependencies instead of Support Library dependencies.

> [!NOTE]
> If your project, and project dependencies, do not reference Android Support Libraries, the migration process does nothing and is not executed.

## Related links

- [How do I migrate my app to Xamarin.Forms 5.0?](~/xamarin-forms/troubleshooting/questions/forms5-migration.md)
- [Android Support Library overview](https://developer.android.com/topic/libraries/support-library/index) on developer.android.com
- [AndroidX overview](https://developer.android.com/jetpack/androidx) on developer.android.com
- [AndroidX class mappings](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-class-mapping.csv)
- [AndroidX assemblies](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-assemblies.csv)
