---
title: "Xamarin.Forms Platform Features"
description: "This guide explains how to take advantage of platform-specific features in Xamarin.Forms applications by using a variety of techniques."
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
---

# Xamarin.Forms Platform Features

Xamarin.Forms is extensible and lets you incorporate platform-specific features using [effects](~/xamarin-forms/app-fundamentals/effects/index.md), [custom renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), the [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), the [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), and more.

## [Android](android/index.md)

This guide describes the Android platform-specifics provided by Xamarin.Forms, and how to implement Material Design by updating existing Xamarin.Forms Android apps.

## [Device class](device.md)

How to use the `Device` class to create platform-specific behavior in shared code and the user interface (including using XAML). Also covers `BeginInvokeOnMainThread` which is essential when modifying UI controls from background threads.

## [iOS](ios/index.md)

This guide describes the iOS platform-specifics provided by Xamarin.Forms, and how to perform additional iOS styling via **Info.plist** and the `UIAppearance` API.

## [Native forms](native-forms.md)

Native Forms allow Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)-derived pages to be consumed by native Xamarin.iOS, Xamarin.Android, and Universal Windows Platform (UWP) projects.

## [Native views](native-views/index.md)

Native views from iOS, Android, and the Universal Windows Platform can be directly referenced from Xamarin.Forms. Properties and event handlers can be set on native views, and they can interact with Xamarin.Forms views.

## [Platform-specifics](platform-specifics/index.md)

Platform-specifics allow you to consume functionality that's only available on a specific platform, without requiring custom renderers or effects. In addition, vendors can create their own platform-specifics with Effects.

## [Windows](windows/index.md)

This guide describes the Windows platform-specifics provided by Xamarin.Forms, and how to add a UWP project to an existing Xamarin.Forms solution.
