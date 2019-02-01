---
title: "Introduction to iOS 12"
description: "This document provides a high-level description of some iOS 12 APIs for which Xamarin's preview release provides C# bindings."
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
---
# Introduction to iOS 12

This document provides a high-level description of some iOS 12 APIs for
which Xamarin's preview release provides C# bindings.

To get started building iOS 12 apps with Xamarin, see the [getting started guide](get-started.md)

## [ARKit 2](arkit2.md)

ARKit is the augmented reality framework included with iOS. ARKit 2 allows
multiple users to interact with each other in an augmented reality scene,
makes it possible to persist objects in space and return to them at a
later time, and provides 2D image recognition and tracking and 3D
object recognition. iOS 12 also provides AR Quick Look, a way to render
usdz AR models in your apps.

## [Siri shortcuts](siri-shortcuts.md)

Siri shortcuts allow developers to more deeply integrate their
applications with Siri. With Siri shortcuts, users can use voice commands
to open content or initiate background tasks, or they can initiate
these same tasks through shortcuts that Siri suggests on the lock
screen.

## [Core ML 2](coreml.md)

Core ML 2 reduces application size through model quantization and flexible
models, improves application performance with a new batch prediction API,
and uses custom models to support advances in machine learning.

## [Notification improvements](notifications/index.md)

In iOS 12, grouped notifications make it possible to present user
notifications in app or thread-related groupings. Summary text provides
further information about a notification group.

Notification content extensions in iOS 12 allow for custom user
interfaces and dynamic action buttons.

## [Natural Language framework](natural-language.md)

The Natural Language framework enables applications to perform various
types of language analysis. For example, it can identify parts of speech
and determine the language represented by a block of text.

## Vision framework

The Vision framework includes an improved face detector that can detect
faces in various orientations. Also, request revisions can select
specific Vision framework algorithm revision.

## Photo and video APIs

In iOS 12, the portrait segmentation API returns a portrait effects
matte – a linear mask that delineates the foreground from the background
of a portrait image and is useful in creating various image effects. iOS 12
also makes it is possible to use depth data from the TrueDepth camera for
real-time video effects.

## Passwords

iOS 12 makes it easier for users and developers to work with passwords:

- Password AutoFill and automatic strong passwords make it possible to
automatically generate, store, and use strong passwords in iOS
applications when signing up for and logging into an application.
- Security Code AutoFill makes it possible to use SMS-based authentication
codes without manual cutting and pasting or memorization.
- The `ASWebAuthenticationSession` class streamlines the process of working
with federated authentication services.
- Autofill Credential Provider extensions make it possible for third-party
password applications to supply username and passwords to login fields.

## HealthKit updates

iOS 11.3 introduced [Health Records](https://www.apple.com/healthcare/health-records/),
which allows users to download their health record information from
various healthcare institutions and view it on their iOS devices. iOS 12
adds APIs that allow third-party applications to securely access this data.

## iMessage App Presentation Contexts

In iOS 12, iMessage apps support presentation contexts, which allow the
apps to run as a normal iMessage app or in the context of a photo or
video effect.

## Network framework

Network framework, the network stack underlying the `URLSession` APIs commonly
used in iOS applications, is now available as a standalone framework,
making it easier to work with TCP, UDP, TLS, IPv4/IPv6, and more.

## CarPlay

In iOS 12, third-party apps can deliver maps and turn-by-turn navigation
instructions in CarPlay by using the new CarPlay framework.

## Deprecations

With iOS 12, Apple has deprecated:

- OpenGL ES, [encouraging developers](https://developer.apple.com/ios/whats-new/)
to adopt Metal.
- [`UIWebView`](xref:UIKit.UIWebView),
[in favor of `WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

## Related links

- [Get Ready for iOS 12 (Apple)](https://developer.apple.com/ios/)
