---
title: "Sharing Code"
description: "Core Application Concepts"
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
---

# Sharing Code

This section provides a guide on some of the more common things tasks or
concepts that developers need to be aware of when developing mobile
applications.

## [Code Sharing Overview](code-sharing.md)

Learn about the different code sharing options available for Xamarin projects, including
Portable Class Libraries (PCLs), Shared Projects, and .NET Standard Libraries.


##  [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md)

Portable Class Library projects let you build and distribute assemblies that contain shared code to run on multiple platforms. To create a Portable Class Library (or "PCL") you first select which platforms to target, then write code against a sub-set of the .NET Framework that is available in the profile defined for those platforms. This document describes how to create and use PCLs with Xamarin.

##  [Shared Projects](~/cross-platform/app-fundamentals/shared-projects.md)

Shared Projects let you write common code that is referenced by a number of different application projects. The code is compiled as part of each referencing project and can include compiler directives to help incorporate platform-specific functionality in the shared code base. This article discusses how Shared Projects work and how to create and use them with Xamarin projects.

##  [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard is a new option for sharing code across platforms. It works in a similar
fashion to Portable Class Libraries; code is built against a specific version (currently 1.0 through 1.6)
and can then be consumed by other projects that support that level or higher. .NET Standard
projects are supported in Xamarin Studio 6.2, Visual Studio for Windows, and Visual Studio for Mac.

##  [NuGet Projects: Multiplatform Libraries for Code Sharing](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet packages can be automatically generated from PCL or .NET standard projects; and Shared Projects can be
packaged into "bait and switch" NuGet packages using the separate NuGet project type. This section explains how
to create NuGet packages for each code-sharing scenario.

##  [Manually Creating NuGet Packages for Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tips for creating NuGet packages that work with the Xamarin platform.
