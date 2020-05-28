---
title: "Styling Xamarin.Forms Apps using XAML Styles"
description: "This guide explains how to customize the appearance of a Xamarin.Forms application by using XAML styles."
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Styling Xamarin.Forms Apps using XAML Styles

## [Introduction](introduction.md)

Xamarin.Forms applications often contain multiple controls that have an identical appearance. Setting the appearance of each individual control can be repetitive and error prone. Instead, styles can be created that customize control appearance by grouping and setting properties available on the control type.

## [Explicit Styles](explicit.md)

An *explicit* style is one that is selectively applied to controls by setting their [`Style`](xref:Xamarin.Forms.NavigableElement.Style) properties.

## [Implicit Styles](implicit.md)

An *implicit* style is one that's used by all controls of the same [`TargetType`](xref:Xamarin.Forms.Style.TargetType), without requiring each control to reference the style.

## [Global Styles](application.md)

Styles can be made available globally by adding them to the application's [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). This helps to avoid duplication of styles across pages or controls.

## [Style Inheritance](inheritance.md)

Styles can inherit from other styles to reduce duplication and enable reuse.

## [Dynamic Styles](dynamic.md)

Styles do not respond to property changes, and remain unchanged for the duration of an application. However, applications can respond to style changes dynamically at runtime by using dynamic resources.

## [Device Styles](device.md)

Xamarin.Forms includes six *dynamic* styles, known as *device* styles, in the [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) class. All six styles can be applied to [`Label`](xref:Xamarin.Forms.Label) instances only.

## [Style Classes](style-class.md)

Xamarin.Forms style classes enable multiple styles to be applied to a control, without resorting to style inheritance.
