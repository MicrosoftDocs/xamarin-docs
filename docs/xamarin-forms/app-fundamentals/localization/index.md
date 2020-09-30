---
title: "Xamarin.Forms Localization"
description: "The built-in .NET localization framework can be used to build cross-platform multilingual applications with Xamarin.Forms. Text and images can be localized, and applications can support a right-to-left flow direction."
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Localization

_The built-in .NET localization framework can be used to build cross-platform multilingual applications with Xamarin.Forms._

## [Xamarin.Forms String and Image Localization](text.md)

The built-in mechanism for localizing .NET applications uses [RESX files](/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) and the classes in the `System.Resources` and `System.Globalization` namespaces. The RESX files containing translated strings are embedded in the Xamarin.Forms assembly, along with a compiler-generated class that provides strongly-typed access to the translations. The translated text can then be retrieved in code.

## [Right-to-Left Localization](right-to-left.md)

Flow direction is the direction in which the UI elements on the page are scanned by the eye. Right-to-left localization adds support for right-to-left flow direction to Xamarin.Forms applications.