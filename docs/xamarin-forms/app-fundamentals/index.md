---
title: "Xamarin.Forms Application Fundamentals"
description: "Exploring the fundamentals of Xamarin.Forms application development, including all the required core concepts, through to finishing touches such as accessibility and localization."
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
---

# Xamarin.Forms Application Fundamentals

## [Accessibility](accessibility/index.md)

Tips to incorporate accessible features (like supporting screen-reading tools) with Xamarin.Forms.

## [App Class](application-class.md)

The `Application` class is the starting point for Xamarin.Forms – every app needs to implement a subclass `App` to set the initial  page. It also provides the `Properties` collection for simple data storage. It can be defined in either C# or XAML.

## [App Lifecycle](app-lifecycle.md)

The `Application` class `OnStart`, `OnSleep`, and `OnResume` methods, as well as modal navigation events, let you handle application lifecycle events with custom code.

## [Application Indexing and Deep Linking](deep-linking.md)

Application indexing allows applications that would otherwise be forgotten after a few uses to stay relevant by appearing in search results. Deep linking allows applications to respond to a search result that contains application data, typically by navigating to a page referenced from a deep link.

## [Behaviors](behaviors/index.md)

User interface controls can be easily extended without subclassing by using behaviors to add functionality.

## [Custom Renderers](custom-renderer/index.md)

Custom Renders let developers 'override' the default rendering of Xamarin.Forms controls to customize their appearance and behavior on each platform (using native SDKs if desired).

## [Data Binding](data-binding/index.md)

Data binding links the properties of two objects, allowing changes in one property to be automatically reflected in the other property. Data binding is an integral part of the Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) application architecture.

## [Dependency Service](dependency-service/index.md)

The `DependencyService` provides a simple locator so that you can code to interfaces in your shared code and provide platform-specific implementations that are automatically resolved, making it easy to reference platform-specific functionality in Xamarin.Forms.

## [Effects](effects/index.md)

Effects allow the native controls on each platform to be customized, and are typically used for small styling changes.

## [Gestures](gestures/index.md)

The Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) class supports tap, pinch, and pan gestures on user interface controls.

## [Localization](localization/index.md)

The built-in .NET localization framework can be used to build cross-platform multilingual applications with Xamarin.Forms.

## [Messaging Center](messaging-center.md)

The Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references

## [Navigation](navigation/index.md)

Xamarin.Forms provides a number of different page navigation experiences, depending upon the `Page` type being used.

## [Shell](shell/index.md)

Xamarin.Forms Shell reduces the complexity of mobile application development by providing the fundamental features that most mobile applications require. This includes a common navigation user experience, a URI-based navigation scheme, and an integrated search handler.

## [Templates](templates/index.md)

Control templates provide the ability to easily theme and re-theme application pages at runtime, while data templates provide the ability to define the presentation of data on supported controls.

## [Triggers](triggers.md)

Update controls by responding to property changes and events in XAML.
