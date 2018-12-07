---
title: "Advanced Concepts & Internals"
description: "This guide introduces advanced concepts and internals for Xamarin.Forms. It currently includes articles about fast renderers and .NET Standard."
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
---

# Advanced Concepts & Internals

## [Dependency Resolution](dependency-resolution.md)

This article explains how to inject a dependency resolution method into Xamarin.Forms so that an application's dependency injection container has control over the creation and lifetime of custom renderers, effects, and `DependencyService` implementations.

## [Fast Renderers](fast-renderers.md)

This article introduces fast renderers, which reduce the inflation and rendering costs of a Xamarin.Forms control on Android by flattening the resulting native control hierarchy.

## [.NET Standard](net-standard.md)

This article explains how to convert a Xamarin.Forms application to use .NET Standard 2.0.
