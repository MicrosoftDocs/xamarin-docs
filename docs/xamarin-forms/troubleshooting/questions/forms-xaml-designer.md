---
title: "Why doesn't the Visual Studio XAML designer work for Xamarin.Forms XAML files?"
ms.topic: article
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
---

# Why doesn't the Visual Studio XAML designer work for Xamarin.Forms XAML files?

Xamarin.Forms doesn't currently support visual designers for XAML files. Because of this, when trying to open a Forms XAML file in either Visual Studio's *XAML UI Designer* or *XAML UI Designer with Encoding*, the following error message is thrown:

> "The file cannot be opened with the selected editor. Please choose another editor."

This limitation is described in the [Overview](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) section of the [Xamarin.Forms XAML Basics](~/xamarin-forms/xaml/xaml-basics/index.md) guide:

> "There is not yet a visual designer for generating XAML in Xamarin.Forms applications, so all XAML must be hand-written."
