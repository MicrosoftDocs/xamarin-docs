---
title: "Ice Cream Sandwich Features"
description: "This article describes several of the new features available to application developers with the Android 4 API - Ice Cream Sandwich. It covers several new user interface technologies and then examines a variety of new capabilities that Android 4 offers for sharing data between applications and between devices."
ms.service: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
---

# Ice Cream Sandwich Features

_This article describes several of the new features available to application developers with the Android 4 API - Ice Cream Sandwich. It covers several new user interface technologies and then examines a variety of new capabilities that Android 4 offers for sharing data between applications and between devices._

## Overview

Android OS version 4.0 (API Level 14) represents a major reworking of the
Android Operating System and includes a number of important changes and
upgrades, including:

- **Updated User Interface** – Several new UI features give developers more power and flexibility when they create application user interfaces. These new features include:  `GridLayout` ,  `PopupMenu` ,  `Switch` widget, and  `TextureView` .
- **Better Hardware Acceleration** – 2D rendering now takes place on the GPU for all Android controls. Additionally, hardware acceleration is on, by default, in all applications developed for Android 4.0.
- **New Data APIs** – There’s new access to data that was not previously officially accessible, such as calendar data and the user profile of the device owner.
- **App Data Sharing** – Sharing data between applications and devices is now easier than ever via technologies such as the  `ShareActionProvider` , which makes it easy to create a sharing action from an Action Bar, and  *Android Beam* for  *Near Field Communications (NFC)* , which makes it a snap to share data across devices in close proximity to each other.

In this article, we’re going to explore these features and other changes
that have been made to the Android 4.0 API, and we’ll explain how to use each
feature with Xamarin.Android.

## User Interface Features

A variety of new user interface technologies are available with Android 4,
including:

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** – Supports 2D grid layout of controls.
- **[Switch widget](~/android/user-interface/controls/switch.md)** – Allows toggling between ON or OFF.
- **[TextureView](~/android/user-interface/controls/texture-view.md)** – Enables video and OpenGL content within a view.
- **[Navigation Bar](~/android/user-interface/controls/navigation-bar.md)** – Contains virtual buttons for back, home, and multi-tasking.

Additionally, other UI elements have been enhanced, such as the `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, which is now easier to work with, and tabs, which have a
more polished appearance.

## Sharing Features

Android 4 includes several new technologies that let us share data across
devices and across applications. It also provides access to various types of
data that were not previously available, such as calendar information and the
device owner’s user profile. In this section we’ll examine a variety of
features offered by Android 4 that address these areas including:

- **[Android Beam](~/android/platform/android-beam.md)** – Allows data sharing via NFC.
- **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)** – Creates a provider that allows developers to specify sharing actions from the Action Bar.
- **[User Profile](~/android/user-interface/user-profile.md)** – Provides access to profile data of the device owner.
- **[Calendar API](~/android/user-interface/controls/calendar.md)** – Provides access to calendar data from the calendar provider.

## x86 Emulators

ICS does not yet support development with an x86 emulator. x86 emulators are
only supported with Android 2.3.3, API level 10. See [Configuring the x86 Emulator](~/android/get-started/installation/android-emulator/index.md) for more information.

## Summary

This article covered a variety of the new technologies that are now available
with Android 4. We reviewed new user interface features such as the *GridLayout*, *PopupMenu*, and *Switch* widget. We also
looked at some of the new support for controlling the system UI, as well as how
to work with the *TextureView*. Then we discussed a variety of new
sharing technologies. We covered how *Android Beam* let’s you share
information across devices that use *NFC*, discussed the new *Calendar API*, and also showed how to use the built in *ShareActionProvider*.
Finally, we examined how to use the *ContactsContract* provider to access
user profile data.

## Related Links

- [TextureViewDemo (sample)](/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (sample)](/samples/xamarin/monodroid-samples/calendardemo)
- [Tab Layout Tutorial](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 Platform](https://developer.android.com/about/versions/android-4.0.html)