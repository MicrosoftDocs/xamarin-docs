---
title: "Xamarin.Essentials: App Theme"
description: "This document describes the Requested App Theme API in Xamarin.Essentials, which provides information as to what theme style is requested for the running app."
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
---

# Xamarin.Essentials: App Theme

The **RequestedTheme** API is part of the [AppInfo](/app-information.md) class and provides information as to what theme is requested for your running app by the system.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using RequestedTheme

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

## Obtaining Theme Information

The requested application theme can be detected with the following API:

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

This will provide the current requested theme by the system for your application. The return value will be one of the following:

* Unspecified
* Light
* Dark

Unspecified will be returned when the operating system does not have a specific user interface style to request. An example of this is on devices running versions of iOS older than 13.0.


## Platform Implementation Specifics

# [Android](#tab/android)

Android uses configuration modes to specify the type of theme to request from the user. Based on the version of Android, it can be changed by the user or is changed when battery saver mode is enabled.

You can read more on the official [Android documentation for Dark Theme](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).


# [iOS](#tab/ios)

Unspecified will always be returned on versions of iOS older than 13.0 


# [UWP](#tab/uwp)

By default your app runs using the theme set by the user in Windows settings (**Settings > Personalization > Colors > Choose your default app mode**). You can set the app's RequestedTheme property to override the user default and specify which theme is used.

You can read more on the [UWP Requested Theme Documentation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme).

--------------

## API

- [AppInfo source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API documentation](xref:Xamarin.Essentials.AppInfo)
