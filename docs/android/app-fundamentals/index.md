---
title: "Xamarin.Android Application Fundamentals"
description: "Core Application Concepts"
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# Xamarin.Android Application Fundamentals

This section provides a guide on some of the more common things tasks or
concepts that developers need to be aware of when developing Android
applications.

## [Accessibility](~/android/app-fundamentals/accessibility.md)

This page describes how to use the Android Accessibility APIs to build
apps according to the
[accessibility checklist](~/cross-platform/app-fundamentals/accessibility.md).

##  [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)

This guide describes how Android uses API levels to manage app
compatibility across different versions of Android, and it explains how
to configure Xamarin.Android project settings to deploy these API
levels in your app. In addition, this guide explains how to write
runtime code that deals with different API levels, and it provides a
reference list of all Android API levels, version numbers (such as
Android 8.0), Android code names (such as Oreo), and build
version codes.



##  [Resources in Android](~/android/app-fundamentals/resources-in-android/index.md)

This article introduces the concept of Android resources in
Xamarin.Android and documents how to use them. It covers how to use
resources in your Android application to support application
localization, and multiple devices including varying screen sizes and
densities.




##  [Activity Lifecycle](~/android/app-fundamentals/activity-lifecycle/index.md)

Activities are a fundamental building block of Android Applications and
they can exist in a number of different states. The activity lifecycle
begins with instantiation and ends with destruction, and includes many
states in between. When an activity changes state, the appropriate
lifecycle event method is called, notifying the activity of the
impending state change and allowing it to execute code to
adapt to that change. This article examines the lifecycle of activities
and explains the responsibility that an activity has during each of
these state changes to be part of a well-behaved, reliable
application.

##  [Localization](~/android/app-fundamentals/localization.md)

This article explains how to localize a Xamarin.Android into other
languages by translating strings and providing alternate images.

## [Services](~/android/app-fundamentals/services/index.md)

This article covers Android services, which are Android components that
allow work to be done in the background. It explains the different
scenarios that services are suited for and shows how to implement them
both for performing long-running background tasks as well as to provide
an interface for remote procedure calls.

## [Broadcast Receivers](~/android/app-fundamentals/broadcast-receivers.md)

This guide covers how to create and use broadcast receivers, an Android
component that responds to system-wide broadcasts, in Xamarin.Android.



##  [Permissions](~/android/app-fundamentals/permissions.md)

You can use the tooling support built into Visual Studio for Mac or Visual
Studio to create and add permissions to the Android Manifest. This
document describes how to add permissions in Visual Studio and Xamarin
Studio.



##  [Graphics and Animation](~/android/app-fundamentals/graphics-and-animation.md)

Android provides a very rich and diverse framework for supporting 2D
graphics and animations. This document introduces these frameworks
and discusses how to create custom graphics and animations and use them
in a Xamarin.Android application.


##  [CPU Architectures](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android supports several CPU architectures, including 32-bit
and 64-bit devices. This article explains how to target an app to one
or more Android-supported CPU architectures.




##  [Handling Rotation](~/android/app-fundamentals/handling-rotation.md)

This article describes how to handle device orientation changes in
Xamarin.Android. It covers how to work with the Android resource system
to automatically load resources for a particular device orientation as
well as how to programmatically handle orientation changes. Then it
describes techniques for maintaining state when a device is rotated.



##  [Android Audio](~/android/app-fundamentals/android-audio.md)

The Android OS provides extensive support for multimedia, encompassing
both audio and video. This guide focuses on audio in Android and covers
playing and recording audio using the built-in audio player and
recorder classes, as well as the low-level audio API. It also covers
working with Audio events broadcast by other applications, so that
developers can build well-behaved applications.




##  [Notifications](~/android/app-fundamentals/notifications/index.md)

This section explains how to implement local and remote notifications in
Xamarin.Android. It describes the various UI elements of an Android
notification and discusses the API's involved with creating and
displaying a notification. For remote notifications, both Google Cloud
Messaging and Firebase Cloud Messaging are explained. Step-by-step
walkthroughs and code samples are included.



##  [Touch](~/android/app-fundamentals/touch/index.md)

This section explains the concepts and details of implementing touch
gestures on Android. Touch APIs are introduced and explained followed
by an exploration of gesture recognizers.



##  [HttpClient Stack and SSL/TLS](~/android/app-fundamentals/http-stack.md)

This section explains the HttpClient Stack and SSL/TLS Implementation
selectors for Android. These settings determine the HttpClient and
SSL/TLS implementation that will be used by your Xamarin.Android apps.


##  [Writing Responsive Applications](writing-responsive-apps.md)

This article discusses how to use threading to keep a Xamarin.Android
application responsive by moving long-running tasks on to a background
thread.