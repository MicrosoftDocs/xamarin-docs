---
title: "Xamarin.Mac performance"
description: "This document provides some performance considerations for Xamarin.Mac apps. It discusses the modern target framework, the linker, AOT, delegates, Cocoa APIs for reusing views, and async code."
ms.service: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
no-loc: [Objective-C]
---

# Xamarin.Mac performance

## Overview

Xamarin.Mac applications are similar to Xamarin.iOS, and many of the same performance suggestions are applicable:

- [Xamarin.iOS performance](~/ios/deploy-test/performance.md)
- [Cross-platform performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)

but there are a number of macOS specific suggestions that may be helpful.

## Prefer modern target framework

There are multiple [Target Frameworks](~/mac/platform/target-framework.md) available to Xamarin.Mac application with different performance characteristics and features.

When possible, prefer Modern and work with dependent libraries to add support. Only the Modern Target Framework allows linking which can drastically reduce assembly sizes. This becomes especially important when enabling AOT, as AOT compilation of Full assemblies can produce large final bundles.

## Enable the linker

Startup time, both in load and "Just In Time" (JIT),  scales somewhat linearly with the size of your final binaries. The easiest way to improve this is by removing dead code with the [linker](~/mac/deploy-test/linker.md).

While this suggestion primarily applies to Modern Target Framework users, use of [Platform Linking](~/mac/deploy-test/linker.md) can provide a limited performance boost as well.

## Enable AOT when appropriate

Another facet of startup performance is the JIT compilation of assemblies into machine code. Ahead of Time (AOT) compilation can significantly reduce startup time, but comes with a number of tradeoffs covered in the [AOT documentation](~/mac/internals/aot.md).

## Ensure performant delegates

Many Xamarin.Mac applications are centered around Cocoa views such as `NSCollectionView`, `NSOutlineView`, or `NSTableView`. Often these views are powered by `Delegate` and `DataSource` classes you provide to Cocoa, answering questions on what to display.

Many of these entry points are invoked often, sometimes multiple times per second when scrolling.

Make sure these functions return values that are easily calculated or use information already cached, to prevent the user interface from blocking.

## Use Cocoa-provided APIs for reusing views

Many Cocoa views that contain many child views or cells (such as `NSCollectionView`, `NSOutlineView`, and `NSTableView`) provide APIs for creating and reusing views. These create pools of shared items and prevent performance issues when scrolling through the views quickly.

## Use async and do not block the UI

Desktop applications often process large quantities of data and it is very easy to block the UI thread waiting on a synchronous operation.

Whenever possible, use [async](~/cross-platform/platform/async.md) and threads to prevent blocking the UI.

For long running operations consider using [NSProgressIndicator](/samples/xamarin/mac-samples/progressbarexample) or other options noted in Apple’s [HIG](https://developer.apple.com/design/human-interface-guidelines/) to notify users.

## Related Links

- [Cross-platform performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Xamarin.iOS performance](~/ios/deploy-test/performance.md)