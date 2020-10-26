---
title: "AndroidX"
description: "How to get started developing apps with AndroidX using Xamarin.Android."
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
---
# AndroidX with Xamarin

_How to get started developing apps with AndroidX using Xamarin.Android._

AndroidX is a major improvement to the original Android Support Library, which is no longer maintained. **AndroidX** packages fully replace the Android Support Library by providing feature parity and new libraries you can use in your Android applications.

AndroidX includes the following features:

- All packages inside AndroidX now have a consistent namespace starting with `androidx`. This means all Android Support Library packages map to a corresponding `androidx.*` package.
- `androidx` packages are separately maintained and updated. This means that you can update AndroidX libraries independently of each other.
- As of v28 of the Android Support Library, there will be no more releases. All development will be included in `androidx` instead.

![AndroidX Logo](~/android/platform/androidx-images/AndroidXLogo.png)

## Requirements

The following list is required to use AndroidX features in Xamarin-based apps:

- **Visual Studio** - On Windows update to Visual Studio 2019 version 16.4 or later. On macOS, update to Visual Studio 2019 for Mac version 8.4 or later.
- **Xamarin.Android** - Xamarin.Android 10.0 or later must be installed with Visual Studio (Xamarin.Android is automatically installed as part of the **Mobile Development With .NET** workload on Windows and installed as part of the **Visual Studio for Mac Installer**)
- **Java Developer Kit** - Xamarin.Android 10.0 development requires JDK 8. Microsoft's distribution of the OpenJDK is automatically installed as part of Visual Studio.
- **Android SDK** - Android SDK API 28 or higher must be installed via the Android SDK Manager.

## Get started

You can get started with AndroidX by including any [AndroidX NuGet package](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22) inside of your Android project. Learn more about installing and using a package in [Visual Studio](/nuget/quickstart/install-and-use-a-package-in-visual-studio) or [Visual Studio for Mac](/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

## Behavior changes

Because AndroidX is a redesign of the Android Support Library, it includes migration steps that will affect Android applications built with the Android Support Library.

### Package Name Change
The package names have been changed between the old and new packages. Below you can see an example of these changes:

| Old                    | New                    |
| ---------------------- | ---------------------- |
| android.support.**     | androidx.@             |
| android.design.**      | com.google.android.material.@ |
| android.support.test.** | androidx.test.@       |
| android.arch.**        | androidx.@             |
| android.arch.persistence.room.** | androidx.room.@ |
| android.arch.persistence.** | androidx.sqlite.@ |

For more details on package naming, [see the following documentation](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings).

## Migration Tooling

There are three migration steps that you'll want to be aware of for your application.

1. If your application includes **Android Support Library namespaces and you'd like to migrate them to AndroidX namespaces**, you can use our **Migrate to AndroidX** IDE tooling to take care of most namespace scenarios. 

Enable the **AndroidX Migrator** via **Tools > Options > Xamarin > Android Settings** inside Visual Studio 2019 (you can skip this step on Visual Studio for Mac).

![Enable AndroidX Migrator](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

Right-click your project and **Migrate to AndroidX**.

![Migrate To AndroidX](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> You will need to make some manual namespace changes for scenarios the tool doesn't cover. While we will map the correct package for you, it is encouraged that you to take a look at the official 
> [artifact mappings](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) and [class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) to help your project migration.

2. If your application includes **any dependencies that have not been migrated to the AndroidX namespace**, you'll have to use the [Android Support Library to AndroidX Migration package.](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)
3. If your application **does not include any dependencies that require AndroidX namespace migration**, you can use the [AndroidX libraries on NuGet today](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22).

## Troubleshooting

- Certain architecture packages within AndroidX will conflict with the Support Library versions. To fix this, you should use the AndroidX version of these packages and remove the Support Library version. For example, if you are referencing `Xamarin.Android.Arch.Work.Runtime` in your project, it will conflict with the types of the newly added `AndroidX.Work` package.

## Summary

This article introduced AndroidX and explained how to install and configure the latest tools and packages for Xamarin.Android development with AndroidX. It provided an overview of what AndroidX is. It included links to API documentation and Android Developer topics to help you get started in 
creating apps using AndroidX. It also highlighted the most important AndroidX behavior changes and troubleshooting topics that could impact existing apps.

## Related links

- [Introduction to AndroidX | The Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin AndroidX GitHub Repository](https://github.com/xamarin/AndroidX)
- [Xamarin AndroidX Migration GitHub Repository](https://github.com/xamarin/XamarinAndroidXMigration)