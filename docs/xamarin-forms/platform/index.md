---
title: "Xamarin.Forms Platform Features"
description: "This guide explains how to take advantage of platform-specific features in Xamarin.Forms applications by using a variety of techniques."
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
---

# Xamarin.Forms Platform Features

Xamarin.Forms is extensible and lets you incorporate platform-specific features using [effects](~/xamarin-forms/app-fundamentals/effects/index.md), [custom renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), the [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), the [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), and more.

## [Android](android/index.md)

This guide describes how to implement Material Design by updating existing Xamarin.Forms Android apps.

## [Application Indexing and Deep Linking](deep-linking.md)

Application indexing allows applications that would otherwise be forgotten after a few uses to stay relevant by appearing in search results. Deep linking allows applications to respond to a search result that contains application data, typically by navigating to a page referenced from a deep link.

## [Device Class](device.md)

How to use the `Device` class to create platform-specific behavior in shared code and the user interface (including using XAML). Also covers `BeginInvokeOnMainThread` which is essential when modifying UI controls from background threads.

## [iOS](ios/index.md)

Some iOS styling can be performed via **Info.plist** and the `UIAppearance` API. This guide includes examples of how to include iOS 9 features into the iOS app of a Xamarin.Forms solution, including Core Spotlight search.

## [GTK](gtk.md)

Xamarin.Forms now has preview support for GTK# apps.

## [Mac](mac.md)

Xamarin.Forms now has preview support for macOS apps.

## [Native Forms](native-forms.md)

Native Forms allow Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)-derived pages to be consumed by native Xamarin.iOS, Xamarin.Android, and Universal Windows Platform (UWP) projects.

## [Native Views](native-views/index.md)

Native views from iOS, Android, and the Universal Windows Platform can be directly referenced from Xamarin.Forms. Properties and event handlers can be set on native views, and they can interact with Xamarin.Forms views.

## [Platform-Specifics](platform-specifics/index.md)

Platform-specifics allow you to consume functionality that's only available on a specific platform, without requiring custom renderers or effects.

## [Plugins](plugins.md)

There are a wide variety of open-source plug-ins available on Github, Nuget, and the Xamarin Component Store to help extend Xamarin.Forms apps.

## [Tizen](tizen.md)

Tizen .NET enables you to build .NET applications with Xamarin.Forms and the Tizen .NET Framework.

## [Windows](windows/index.md)

Xamarin.Forms has support for the Universal Windows Platform (UWP) on Windows 10. This article describes how to add a a UWP project to an existing Xamarin.Forms solution.

## [WPF](wpf.md)

Xamarin.Forms now has preview support for Windows Presentation Foundation (WPF) apps.
