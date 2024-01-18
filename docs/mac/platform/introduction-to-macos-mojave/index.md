---
title: "Introduction to macOS Mojave"
description: "This document provides a high-level of overview of new and updated features in macOS Mojave."
ms.service: xamarin
ms.assetid: 4A41CD85-C807-44C9-85AB-B5441B145A73
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
no-loc: [Objective-C]
---
# Introduction to macOS Mojave

This document provides a high-level of overview of new and updated
features in macOS Mojave.

To get started building macOS Mojave apps with Xamarin, take a look at the [getting started guide](~/mac/platform/introduction-to-macos-mojave/get-started.md) for [Xamarin.Mac 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/mac/xamarin.mac_5/xamarin.mac_5.0.md).

## Dark Mode

Dark mode is a system-wide dark theme in macOS Mojave that uses a dynamic,
dark grey color scheme to display user interface elements. It also
introduces new accent colors, color effects, and content tint colors to
help third-party apps look good no matter the user's color settings.

## User Notifications framework

The User Notifications framework is included in macOS Mojave, changing
the APIs that Mac apps use to work with user notifications.

## Natural Language framework

The Natural Language framework enables applications to perform various
types of language analysis. For example, it can be used to identify parts
of speech and determine the language represented by a block of text.

## Vision framework

The Vision framework includes an improved face detector that can detect
faces in various orientations. Also, request revisions can now be used to
select a specific Vision framework algorithm revision.

## Network framework

Network framework, the network stack underlying the `URLSession` APIs
commonly used in iOS applications, is now available as a standalone
framework, making it easier to work with TCP, UDP, TLS, IPv4/IPv6, and
more.

## Deprecations

With macOS Mojave, Apple has deprecated OpenGL ES and OpenCL,
[encouraging developers](https://developer.apple.com/macos/whats-new/)
to adopt Metal and Metal Performance Shaders.

## Related links

- [Xamarin.Mac samples](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [macOS – Apple Developer](https://developer.apple.com/macos/)
- [Xamarin.Mac 5.0 release notes](/xamarin/mac/release-notes/5/5.0/)