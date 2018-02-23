---
title: "Styles"
description: "Using styles to customize appearance"
ms.topic: article
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
---

# Styles

## [Introduction](introduction.md)

Xamarin.Forms applications often contain multiple controls that have an identical appearance. Setting the appearance of each individual control can be repetitive and error prone. Instead, styles can be created that customize control appearance by grouping and settings properties available on the control type.

## [Explicit Styles](explicit.md)

An *explicit* style is one that is selectively applied to controls by setting their [`Style`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) properties.

## [Implicit Styles](implicit.md)

An *implicit* style is one that's used by all controls of the same [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), without requiring each control to reference the style.

## [Global Styles](application.md)

Styles can be made available globally by adding them to the application's [`ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). This helps to avoid duplication of styles across pages or controls.

## [Style Inheritance](inheritance.md)

Styles can inherit from other styles to reduce duplication and enable reuse.

## [Dynamic Styles](dynamic.md)

Styles do not respond to property changes, and remain unchanged for the duration of an application. However, applications can respond to style changes dynamically at runtime by using dynamic resources.

## [Device Styles](device.md)

Xamarin.Forms includes six *dynamic* styles, known as *device* styles, in the [`Devices.Styles`](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) class. All six styles can be applied to [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances only.
