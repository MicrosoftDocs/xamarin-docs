---
title: "Xamarin.Essentials: App Information"
description: "This document describes the AppInfo class in Xamarin.Essentials, which provides information about your application. For example, it exposes the app name and version."
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: App Information

![Pre-release NuGet](~/media/shared/pre-release.png)

The **AppInfo** class provides information about your application.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using AppInfo

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

## Obtaining Application Information:

The following information is exposed through the API:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## Displaying Application Settings

The **AppInfo** class can also display a page of settings maintained by the operating system for the application:

```csharp
// Display settings page
AppInfo.ShowSettingsUI();
```

This settings page allows the user to change application permissions and perform other platform-specific tasks.

## API

- [AppInfo source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API documentation](xref:Xamarin.Essentials.AppInfo)
