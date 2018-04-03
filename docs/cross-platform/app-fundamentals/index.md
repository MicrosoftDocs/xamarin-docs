---
title: "Application Fundamentals"
description: "Core Application Concepts"
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
---

# Application Fundamentals

This section provides a guide on some of the more common things tasks or
concepts that developers need to be aware of when developing mobile
applications.

##  [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

By choosing Xamarin and keeping a few things in mind when you design and
develop your mobile applications, you can realize tremendous code sharing across
mobile platforms, reduce your time to market, leverage existing talent, meet
customer demand for mobile access, and reduce cross-platform
complexity.&nbsp;This document outlines key guidelines to realizing these
advantages for utility and productivity applications.

## [Code Sharing Options](code-sharing.md)

Learn about the different code sharing options available for Xamarin projects, including
Portable Class Libraries (PCLs), Shared Projects, and .NET Standard Libraries.


## [Accessibility](accessibility.md)

Tips for building accessible applications.


## [Localization](localization.md)

Guidelines for making locale-aware apps that can be translated into multiple languages.


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

##  [Cross Platform Data Access](~/xamarin-forms/data-cloud/index.md)

Most applications have some requirement to save data on the device locally. Unless the amount of data is trivially small, this usually requires a database and a data layer in the application to manage database access. iOS and Android both have the SQLite database engine “built in” and access to store and retrieve data is simplified by Xamarin’s platform. The [Android data access](~/android/data-cloud/data-access/index.md), [iOS data access](~/ios/data-cloud/data/index.md), and [Xamarin.Forms data access](~/xamarin-forms/data-cloud/index.md) guides provide examples of how to access SQLite on each platform.


##  [Transport Layer Security](transport-layer-security.md)

Information on selectingthe correct SSL/TLS implementation to secure
your app's network connectivity.


##  [Notifications](~/xamarin-forms/data-cloud/push-notifications/index.md)

Mobile applications use notifications as an unobtrusive way of informing the user that some application specific
event has happened. Notifications are typically used to notify the user of the status of an application process that
is running in the background. An example of this might be downloading a large file. This file might take a long time
to download, so this activity should occur in the background. When the download is complete, the user is informed of
the fact by a notification.
Additionally, notification ares not just limited to local applications. It is also possible for server applications
to publish notifications to mobile applications. This article will discuss how to use notifications on both Android
and iOS.
