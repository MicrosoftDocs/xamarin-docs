---
title: "Under the hood in Xamarin.Mac"
description: "This document links to various guides that describe the inner workings of Xamarin.Mac. Linked documents discuss ahead of time compilation, Xamarin.Mac architecture, and the Xamarin.Mac registrar."
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
---

# Under the hood in Xamarin.Mac

## [Ahead of time compilation (AOT)](aot.md)

Ahead of time (AOT) compilation is a powerful optimization technique for improving startup performance. However, it also affects your build time, application size, and program execution in profound ways, so it's worthwhile understanding how it works.

## [Mac architecture](architecture.md)

Xamarin.Mac's relationship to Objective-C, including concepts such as compilation, selectors, registrars, app launch, and the generator.

## [Xamarin.Mac registrar](registrar.md)

Xamarin.Mac bridges the gap between the managed world and Cocoa's runtime, allowing managed classes to call unmanaged Objective-C classes and be called back when events occur. The work required to preform this “magic” is handled by the registrar, but understanding what's going on "under the hood" can sometimes be helpful.
