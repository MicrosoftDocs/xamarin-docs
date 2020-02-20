---
title: "Xamarin.Forms Dual-Screen"
description: "This guide explains how to create Xamarin.Forms apps for dual-screen devices."
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
---

# Xamarin.Forms dual-screen

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo (Android) and Surface Neo (Windows 10X) introduce new patterns for touch applications. Xamarin.Forms includes `TwoPaneView` and `DualScreenInfo` classes so you can develop apps for these devices.

## [Dual-Screen Patterns](design-patterns.md)

When considering how to best utilize multiple screens on a dual-screen device, refer to this pattern guidance to find the best fit for your application interface.

## [TwoPaneView](twopaneview.md)

The Xamarin.Forms `TwoPaneView` class, inspired by the UWP control of the same name, is a cross-platform layout optimized for dual-screen devices.

## [DualScreenInfo](dual-screen-info.md)

The `DualScreenInfo` class enables you to determine which pane your view is on, how big it is, what posture the device is in, the angle of the hinge, and more.

## [DualScreenHelper](dual-screen-helper.md)

The `DualScreenHelper` class lets you check if the platform supports opening a new window as Picture in Picture. For Neo, this enables you to open a window that will display in the Wonderbar, when the device is in compose mode.
