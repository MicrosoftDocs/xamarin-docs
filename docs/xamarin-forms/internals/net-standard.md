---
title: ".NET Standard 2.0 Support in Xamarin.Forms"
description: "This article explains how to convert a Xamarin.Forms application to use .NET Standard 2.0."
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
---

# .NET Standard 2.0 Support in Xamarin.Forms

_This article explains how to convert a Xamarin.Forms application to use .NET Standard 2.0._

.NET Standard is a specification of .NET APIs that are intended to be available on all .NET implementations. It makes it easier to share code across desktop applications, mobile apps and games, and cloud services, by bringing identical APIs to the different platforms. For information about the platforms supported by .NET Standard, see [.NET implementation support](/dotnet/standard/net-standard#net-implementation-support/).

.NET Standard libraries are the replacement for Portable Class Libraries (PCL). However, a library that targets .NET Standard is still a PCL, and is referred to as a .NET Standard-based PCL. Certain PCL profiles are mapped to .NET Standard versions, and for profiles that have a mapping, the two library types will be able to reference each other. For more information, see [PCL compatibility](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 allows Xamarin.Forms applications to target .NET Standard 2.0 by replacing the PCL with a .NET Standard 2.0 library. This can be achieved as follows:

- Ensure [.NET Core 2.0](https://www.microsoft.com/net/download/core) is installed.
- Update the Xamarin.Forms solution to use Xamarin.Forms 2.4, or greater.
- Add a .NET Standard library to the solution, that targets .NET Standard 2.0.
- Delete the class that's added to the .NET Standard library.
- Add the Xamarin.Forms 2.4 (or greater) NuGet package to the .NET Standard library.
- In the platform projects, add a reference to the .NET Standard library and remove the reference to the PCL project that contains the Xamarin.Forms user interface logic.
- Copy the files from the PCL project to the .NET Standard library.
- Remove the PCL project that contains the Xamarin.Forms user interface logic.


## Related Links

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Code Sharing Options](~/cross-platform/app-fundamentals/code-sharing.md)
