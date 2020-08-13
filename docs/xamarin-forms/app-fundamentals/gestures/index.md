---
title: "Xamarin.Forms gestures"
description: "This guide explains how Xamarin.Forms gesture recognizers can be used to detect user interaction with views in a Xamarin.Forms application."
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/04/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms gestures

_Gesture recognizers can be used to detect user interaction with views in a Xamarin.Forms application._

The Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) class supports tap, pinch, pan, swipe, and drag and drop gestures on [`View`](xref:Xamarin.Forms.View) instances.

## [Add a tap gesture recognizer](tap.md)

A tap gesture is used for tap detection and is recognized with the [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) class.

## [Add a pinch gesture recognizer](pinch.md)

A pinch gesture is used for performing interactive zoom and is recognized with the [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) class.

## [Add a pan gesture recognizer](pan.md)

A pan gesture is used for detecting the movement of fingers around the screen and applying that movement to content, and is recognized with the [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) class.

## [Add a swipe gesture recognizer](swipe.md)

A swipe gesture occurs when a finger is moved across the screen in a horizontal or vertical direction, and is often used to initiate navigation through content. Swipe gestures are recognized with the [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) class.

## [Add a drag and drop gesture recognizer](drag-and-drop.md)

A drag and drop gesture enables items, and their associated data packages, to be dragged from one onscreen location to another location using a continuous gesture. Drag gestures are recognized with the `DragGestureRecognizer` class, and drop gestures are recognized with the `DropGestureRecognizer` class.
