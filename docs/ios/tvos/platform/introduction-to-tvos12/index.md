---
title: "Introduction to tvOS 12"
description: "This document provides a high-level of overview of new and updated features in tvOS 12 for which Xamarin's preview release currently provides C# bindings."
ms.service: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
no-loc: [Objective-C]
---
# Introduction to tvOS 12

This document provides a high-level overview of new and updated tvOS 12.

To get started building tvOS 12 apps with Xamarin, take a look at the [getting started guide](~/ios/platform/introduction-to-ios12/get-started.md).

## TVUIKit

tvOS 12 includes TVUIKit, a set of APIs that make it possible for tvOS
developers to use common tvOS controls such as poster views, caption
buttons, card views, and monogram views. tvOS 12 also introduces a
property that allows labels to scroll text that is too long to be
completely visible.

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

## Vision framework

The Vision framework includes an improved face detector that can detect
faces in various orientations. Also, request revisions can now be used to
select a specific Vision framework algorithm revision.

## Natural Language framework

The Natural Language framework enables applications to perform various
types of language analysis. For example, it can be used to identify parts
of speech and determine the language represented by a block of text.

## Deprecations

With tvOS 12, Apple has deprecated OpenGL ES,
[encouraging developers](https://developer.apple.com/tvos/whats-new/)
to adopt Metal.

## Related links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [What's new in tvOS 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)