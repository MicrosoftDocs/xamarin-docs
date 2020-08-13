---
title: "Xamarin.Forms App Lifecycle"
description: "This article explains how to respond to the application lifecycle, including lifecycle methods, page notification events, and modal navigation events."
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms App Lifecycle

The [`Application`](xref:Xamarin.Forms.Application) base class provides the following features:

- [Lifecycle methods](#lifecycle-methods) `OnStart`, `OnSleep`, and `OnResume`.
- [Page navigation events](#page-navigation-events) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing).
- [Modal navigation events](#modal-navigation-events) `ModalPushing`, `ModalPushed`, `ModalPopping`, and `ModalPopped`.

## Lifecycle methods

The [`Application`](xref:Xamarin.Forms.Application) class contains three virtual methods that can be overridden to respond to lifecycle changes:

- `OnStart` - called when the application starts.
- `OnSleep` - called each time the application goes to the background.
- `OnResume` - called when the application is resumed, after being sent to the background.

> [!NOTE]
> There is *no* method for application termination. Under normal circumstances (i.e. not a crash) application termination will happen from the *OnSleep* state, without any additional notifications to your code.

To observe when these methods are called, implement a `WriteLine` call in each (as shown below) and test on each platform.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

> [!IMPORTANT]
> On Android, the `OnStart` method will be called on rotation as well as when the application first starts, if the main activity lacks `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` in the `[Activity()]` attribute.

## Page navigation events

There are two events on the [`Application`](xref:Xamarin.Forms.Application) class that provide notification of pages appearing and disappearing:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - raised when a page is about to appear on the screen.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - raised when a page is about to disappear from the screen.

These events can be used in scenarios where you want to track pages as they appear on screen.

> [!NOTE]
> The [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) and [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) events are raised from the [`Page`](xref:Xamarin.Forms.Page) base class immediately after the [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) and [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) events, respectively.

## Modal navigation events

There are four events on the [`Application`](xref:Xamarin.Forms.Application) class, each with their own event arguments, that let you respond to modal pages being shown and dismissed:

- `ModalPushing` - raised when a page is modally pushed.
- `ModalPushed` - raised after a page has been pushed modally.
- `ModalPopping` - raised when a page is modally popped.
- `ModalPopped` - raised after a page has been popped modally.

> [!NOTE]
> The `ModalPopping` event arguments, of type `ModalPoppingEventArgs`, contain a `Cancel` property. When `Cancel` is set to `true` the modal pop is cancelled.
