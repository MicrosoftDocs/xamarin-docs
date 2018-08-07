---
title: "Introduction to iOS 12"
description: "This document provides a high-level description of some iOS 12 APIs for which Xamarin's preview release provides C# bindings."
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
---
# Introduction to iOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Xamarin's iOS 12 support is currently in preview, which means that it
> may contain bugs, is not feature-complete, and may change. Use it for
> experimentation only.

This document provides a high-level description of some iOS 12 APIs for
which Xamarin's preview release provides C# bindings.

To get started building iOS 12 apps with Xamarin, take a look at:

- The [getting started guide](get-started.md)
- The Xamarin preview [release blog post](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)

## ARKit 2

ARKit is the augmented reality framework included with iOS. ARKit 2 allows
multiple users to interact with each other in an augmented reality scene,
makes it possible to persist objects in space and return to them at a
later time, and provides 2D image recognition and tracking and 3D
object recognition. iOS 12 also provides AR Quick Look, a way to render
usdz AR models in your apps.

## Siri shortcuts

Siri shortcuts allow developers to more deeply integrate their
applications with Siri. With Siri shortcuts, users can use voice commands
to open content or kick off tasks in their apps. Siri will learn when
certain shortcuts are more likely to be used and suggest them to the user
via notifications.

## Core ML 2

Core ML 2 reduces application size through model quantization and flexible
models, improves application performance with a new batch prediction API,
and uses custom models to support advances in machine learning.

## Notification improvements

In iOS 12, grouped notifications make it possible to present user
notifications in app or thread-related groupings. Summary text can be
used to provide further information about a notification group.

Notification content extensions in iOS 12 allow for custom user
interfaces and dynamic actions. These features enable richer, more
relevant experiences in user notifications.

## Natural Language framework

The Natural Language framework enables applications to perform various
types of language analysis. For example, it can be used to identify parts
of speech and determine the language represented by a block of text.

## CarPlay

In iOS 12, third-party apps can deliver maps and turn-by-turn navigation
instructions in CarPlay by using the new CarPlay framework.

## Automatic strong passwords

iOS 12 will suggest and store usernames and strong passwords for
applications containing a create account screen. The suggested passwords
can be generated based on a default, 20-character format or based on
developer-specified password rules. This feature makes use of associated
domains and specified content types on new username and new password
fields.

## AutoFill Credential Provider Extensions

With iOS 12, third-party password manager applications can provide an
extension to supply username and password values to login fields.

## HealthKit updates

iOS 11.3 introduced [Health Records](https://www.apple.com/healthcare/health-records/),
which allows users to download their health record information from
various healthcare institutions and view it on their iOS devices. iOS 12
adds APIs that allow third-party applications to securely access this data.

## iMessage App Presentation Contexts

In iOS 12, iMessage apps support presentation contexts, which allow the
apps to run as a normal iMessage app or in the context of a photo or
video effect.

## Vision framework

The Vision framework includes an improved face detector that can detect
faces in various orientations. Also, request revisions can now be used to
select a specific Vision framework algorithm revision.

## Related links

- [Get Ready for iOS 12 (Apple)](https://developer.apple.com/ios/)
- [iOS 12 Preview (Apple)](https://www.apple.com/ios/ios-12-preview/)
- Xamarin preview [release blog post](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
