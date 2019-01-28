---
title: "Sharing Code On Multiple Platforms"
description: "This document links to various guides that describe techniques for sharing code, including portable class libraries, shared projects, .NET Standard, and NuGet."
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
---
# Sharing code on multiple platforms

These articles explain the different options available for sharing code across platforms, including Windows, Android, iOS, and more.

## [Code sharing overview](code-sharing.md)

Learn about the different code sharing options available for Xamarin projects, including
.NET Standard Libraries and Shared Projects. Portable Class Libraries are also supported,
however they are considered deprecated in favor of .NET Standard.

## [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard is the preferred option for sharing code across platforms. Code is built against a
specific version (2.0 provides the best API compatibility with existing .NET Framework code)
and can then be consumed by other projects that support that level or higher. .NET Standard
projects are supported in both Visual Studio 2017 and Visual Studio for Mac.

## [Shared Projects](~/cross-platform/app-fundamentals/shared-projects.md)

Shared Projects let you write common code that is referenced by a number of different application projects. The code is compiled as part of each referencing project and can include compiler directives to help incorporate platform-specific functionality in the shared code base. This article discusses how Shared Projects work and how to create and use them with Xamarin projects.

## [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md)

Portable Class Library projects let you build and distribute assemblies that contain shared code to run on multiple platforms. To create a Portable Class Library (or "PCL") you first select which platforms to target, then write code against a sub-set of the .NET Framework that is available in the profile defined for those platforms. PCLs are considered to be deprecated in the latest versions of Visual Studio; developers are encouraged to use .NET Standard 2.0 instead.

## [NuGet projects: Multiplatform Libraries for code sharing](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet packages can be automatically generated from PCL or .NET standard projects; and Shared Projects can be
packaged into "bait and switch" NuGet packages using the separate NuGet project type. This section explains
how to create NuGet packages for each code-sharing scenario.

## [Manually creating NuGet packages for Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tips for creating NuGet packages that work with the Xamarin platform.

## [Use C/C++ Libraries in Cross-Platform Xamarin Projects](~/cross-platform/cpp/index.md)

This technique allows you to decouple the evolution of your C/C++ libraries, a C# binding in a NuGet, and 
your Xamarin applications. Functionality is provided by the native-platform C/C++ library, but all 
platform-specific code is isolated from the final Xamarin application(s), allowing the highest-possible 
performance with no code duplication. 
