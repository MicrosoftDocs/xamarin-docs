---
title: "Platform Features"
description: "Take advantage of platform-specific features with Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
---

# Platform Features

Xamarin.Forms is extensible and lets you incorporate platform-specific features using [effects](~/xamarin-forms/app-fundamentals/effects/index.md), [custom renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), the [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), the [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), and more.

## [Android](android/index.md)

This guide describes how to implement Material Design by updating existing Xamarin.Forms Android apps.

## [Application Indexing and Deep Linking](deep-linking.md)

Application indexing allows applications that would otherwise be forgotten after a few uses to stay relevant by appearing in search results. Deep linking allows applications to respond to a search result that contains application data, typically by navigating to a page referenced from a deep link.

## [Device Class](device.md)

How to use the `Device` class to create platform-specific behavior in shared code and the user interface (including using XAML). Also covers `BeginInvokeOnMainThread` which is essential when modifying UI controls from background threads.

## [iOS](ios/index.md)

Some iOS styling can be performed via **Info.plist** and the `UIAppearance` API. This guide includes examples of how to include iOS 9 features into the iOS app of a Xamarin.Forms solution, including Core Spotlight search.

## [Mac](mac.md)

Xamarin.Forms now has preview support for macOS apps.

## [Native Forms](native-forms.md)

Native Forms allow Xamarin.Forms [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-derived pages to be consumed by native Xamarin.iOS, Xamarin.Android, and Universal Windows Platform (UWP) projects.

## [Native Views](native-views/index.md)

Native views from iOS, Android, and the Universal Windows Platform can be directly referenced from Xamarin.Forms. Properties and event handlers can be set on native views, and they can interact with Xamarin.Forms views.

## [Platform-Specifics](platform-specifics/index.md)

Platform-specifics allow you to consume functionality that's only available on a specific platform, without requiring custom renderers or effects.

## [Plugins](plugins.md)

There are a wide variety of open-source plug-ins available on Github, Nuget, and the Xamarin Component Store to help extend Xamarin.Forms apps.

## [Windows](windows/index.md)

Xamarin.Forms has support for four different types of Windows project:

* Windows Phone 8 Silverlight (the original Windows platform supported by Xamarin.Forms),
* Windows Phone 8.1 (WinRT),
* Windows 8.1 (WinRT), and
* Universal Windows Platform (Windows 10).

This section describes the differences between them, and how to add them to an existing Xamarin.Forms solution.
