---
title: "Xamarin.Forms App Lifecycle"
description: "This article explains how to respond to the application lifecycle, including lifecycle methods, page navigation events, and modal navigation events."
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
---

# Xamarin.Forms App Lifecycle

The [`Application`](xref:Xamarin.Forms.Application) base class offers the following features:

* [Lifecycle methods](#Lifecycle_Methods) `OnStart`, `OnSleep`, and `OnResume`.
* [Page navigation events](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing).
* [Modal navigation events](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, and `ModalPopped`.

<a name="Lifecycle_Methods" />

## Lifecycle Methods

The [`Application`](xref:Xamarin.Forms.Application) class contains three virtual methods that
  can be overridden to handle lifecycle methods:

* **OnStart** - Called when the application starts.

* **OnSleep** - Called each time the application goes to the background.

* **OnResume** - Called when the application is resumed, after being sent to the background.

Note that there is *no* method for application termination.
  Under normal circumstances (i.e. not a crash) application
  termination will happen from the *OnSleep* state, without
  any additional notifications to your code.

To observe when these methods are called, implement a `WriteLine`
  call in each (as shown below) and test on each platform.

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

When updating *older* Xamarin.Forms applications (eg. create with Xamarin.Forms 1.3 or older),
ensure that the Android
main activity includes `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation`
in the `[Activity()]` attribute. If this is not present you will observe the `OnStart` method gets
called on rotation as well as when the application first starts. This attribute is automatically
included in the current Xamarin.Forms app templates.

<a name="page" />

## Page Navigation events

There are two events on the [`Application`](xref:Xamarin.Forms.Application) class that provide notification of pages appearing and disappearing:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - raised when a page is about to appear on the screen.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - raised when a page is about to disappear from the screen.

These events can be used in scenarios where you want to track pages as they are appearing on screen.

> [!NOTE]
> The [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) and [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) events are raised from the [`Page`](xref:Xamarin.Forms.Page) base class immediately after the [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) and [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) events, respectively.

<a name="modal" />

## Modal Navigation Events

There are four events on the [`Application`](xref:Xamarin.Forms.Application) class, each with their own event arguments, that let you respond to modal pages being shown and dismissed:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - the `ModalPoppingEventArgs` class contains a
  `Cancel` property. When `Cancel` is set to `true` the modal pop
  is cancelled.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> To implement the application lifecycle methods and modal navigation events,
> all pre-`Application` methods
> of creating a Xamarin.Forms app (i.e. applications written in version
> 1.2 or older that use a static `GetMainPage` method)
> have been updated to create a default `Application` which is
> set as the parent of `MainPage`.
>
> Xamarin.Forms applications that use this legacy behavior must be updated to
> an `Application` subclass as described on the
> [Application class](~/xamarin-forms/app-fundamentals/application-class.md) page.
