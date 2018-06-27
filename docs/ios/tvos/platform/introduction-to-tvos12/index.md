---
title: "Introduction to tvOS 12"
description: "This document provides a high-level of overview of new and updated features in tvOS 12 for which Xamarin's preview release currently provides C# bindings."
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
---
# Introduction to tvOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Xamarin's tvOS 12 support is currently in preview, which means that it
> may contain bugs, is not feature-complete, and may change. Use it for
> experimentation only.

> [!NOTE]
> - Review the [getting started](~/ios/platform/introduction-to-ios12/get-started.md)
>   guide for instructions on how to get started building iOS 12 and tvOS
>   12 apps with Xamarin.
> - For more information, read the
>   [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
>   for the Xamarin preview release.

This document provides a high-level overview of new and updated tvOS 12
features for which Xamarin's preview release currently provides C# bindings.

## Password AutoFill

With tvOS 12, users can use their iOS devices to sign in to a tvOS app with
a single tap. This is enabled through a combination of `UITextContentType`
usage to specify username and password fields, associated domains to
establish a relationship between an iOS app and a tvOS app, and preferred
focus environments to select an item to receive focus after a user
provides a username and password.

## Focus Engine enhancements

tvOS 12 allows all apps, no matter how they are rendered, to interact
with the Focus Engine. Through a user's interactions with the Siri
Remote, the Focus Engine can be used with any app to select an item, hint
at possible focus changes, and naturally update focus. This is enabled in
custom applications through UIKit's `IUIFocusItemContainer` interface,
the `UIFocusMovementHint` class, the `IUIFocusItemScrollableContainer`
interface, and other related classes and methods.

## Related links

- [tvOS Samples](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [What's new in tvOS 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin preview [release notes](https://releases.xamarin.com/preview-release-xcode-10-beta/)
