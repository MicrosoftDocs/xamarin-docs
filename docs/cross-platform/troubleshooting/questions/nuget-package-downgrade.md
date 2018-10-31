---
title: "How do I downgrade a NuGet package?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
---

# How do I downgrade a NuGet package?

Visual Studio for Mac & Visual Studio both have features for selecting older versions of packages and installing them automatically; similar to how updating packages works. These steps are described below.

## Visual Studio

1. Go to **Tools > NuGet Package Manager > Package Manager Console**
2. Set the project under **Default Project**
3. Use this syntax:

    > Install-Package [PackageName] -Version [tab for version menu]

You can also copy/paste the exact command from the package's NuGet page. Example for Xamarin.Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## Visual Studio for Mac

1. In your project, right-click the packages folder & select **Add Packages**
2. In the searchbar, you can use the following syntax to search for your required packages:

    `[PackageName] version:*`

### Examples 
- Lists all Xamarin.Forms packages: 

    `Xamarin.Forms version:`

- Lists all Xamarin.Forms 1.4.x packages: 

    `Xamarin.Forms version:1.4`

*Note: If you add a space between `version:` & the version number, the search will behave as though no version was specified.*
