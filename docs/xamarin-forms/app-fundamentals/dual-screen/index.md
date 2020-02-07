---
title: "Xamarin.Forms Dual-Screen Features"
description: "This guide explains how Xamarin.Forms can easily enlighten apps for dual-screen devices."
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
---

# Xamarin.Forms dual-screen

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo (Android) and Surface Neo (Windows 10X) introduce new patterns for touch applications. Xamarin.Forms introduces `TwoPaneView` and `DualScreenInfo` to empower you to take full advantage of these new experiences.

## [Dual-Screen Patterns](design-patterns.md)

When considering how to best utilize multiple screens on a dual-screen device, refer to our pattern guidance to find the best fit for your application interface.

## [TwoPaneView](twopaneview.md)

Xamarin.Forms `TwoPaneView` class, inspired by the UWP control of the same name, introduces a layout optimized for dual-screen devices in a cross-platform way.

## [DualScreenInfo](dual-screen-info.md)

To get the most out of dual-screen devices, you may want to know which pane your view is on, how big it is, what posture the device is in, the angle of the hinge, and more. `DualScreenInfo` provides this information.
