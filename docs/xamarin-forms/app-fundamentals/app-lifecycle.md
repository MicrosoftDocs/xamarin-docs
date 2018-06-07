---
title: "App Lifecycle"
description: "How to respond to application lifecycle"
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
---

# App Lifecycle

The `Application` base class offers the following features:

* [Lifecycle methods](#Lifecycle_Methods) `OnStart`, `OnSleep`, and `OnResume`.
* [Modal navigation events](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, and `ModalPopped`.

<a name="Lifecycle_Methods" />

## Lifecycle Methods

The `Application` class contains three virtual methods that
  can be overridden to handle lifecycle methods:

* **OnStart** - Called when the application starts.

* **OnSleep** - Called each time the application goes to the background.

* **OnResume** - Called when the application is resumed, after being sent to the background.

Note that there is *no* method for application termination.
  Under normal circumstances (ie. not a crash) application
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

<a name="modal" />

## Modal Navigation Events

There are four new events on the `Application` class in Xamarin.Forms 1.4,
  each with their own event arguments:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - the `ModalPoppingEventArgs` class contains a
  `Cancel` property. When `Cancel` is set to `true` the modal pop
  is cancelled.
* **ModalPopped** - `ModalPoppedEventArgs`

These events will help you better manage your application lifecycle, by
  letting you respond to modal pages being shown and dismissed.

> [!NOTE]
> To implement the application lifecycle methods and modal navigation events,
> all pre-`Application` methods
> of creating a Xamarin.Forms app (ie. applications written in version
> 1.2 or older that use a static `GetMainPage` method)
> have been updated to create a default `Application` which is
> set as the parent of `MainPage`.
>
> Xamarin.Forms applications that use this legacy behavior must be updated to
> an `Application` subclass as described on the
> [Application class](~/xamarin-forms/app-fundamentals/application-class.md) page.
