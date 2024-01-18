---
title: "Providing Haptic Feedback in Xamarin.iOS"
description: "This document describes how to provide haptic feedback in a Xamarin.iOS app. It discusses UIImpactFeedbackGenerator, UINotificationFeedbackGenerator, and UISelectionFeedbackGenerator."
ms.service: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# Providing Haptic Feedback in Xamarin.iOS

<a name="Overview"></a>

## Overview

On the iPhone 7 and iPhone 7 Plus, Apple has included new haptic responses that provide additional ways to physically engage the user. Haptic Feedback (often referred to simply as Haptics) uses the sense of touch (via force, vibrations or motion) in User Interface design. Use these new tactile feedback options to get the user's attention and reinforce their actions.

The following topics will be covered in detail:

- [About Haptic Feedback](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback"></a>

## About Haptic Feedback

Several built-in UI elements already provide haptic feedback such as Pickers, Switches and Sliders. iOS 10 now adds the ability to programmatically trigger haptics using a concrete subclass of the `UIFeedbackGenerator` class.

The developer can use one of the following `UIFeedbackGenerator` subclasses to programmatically trigger haptic feedback:

- `UIImpactFeedbackGenerator` - Use this feedback generator to complement an action or task such as presenting a "thud" when a View slides into place or if two onscreen objects collide.
- `UINotificationFeedbackGenerator` - Use this feedback generator for notifications such as an action completing, failing or any other type of warning.
- `UISelectionFeedbackGenerator` - Use this feedback generator for a selection actively changing such as picking an item from a list.

<a name="UIImpactFeedbackGenerator"></a>

### UIImpactFeedbackGenerator

Use this feedback generator to complement an action or task such as presenting a "thud" when a View slides into place or if two onscreen objects collide.

Use the following code to trigger impact feedback:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

When the developer creates a new instance of the `UIImpactFeedbackGenerator` class they provide a `UIImpactFeedbackStyle` specifying the strength of the feedback as:

- `Heavy`
- `Medium`
- `Light`

The `Prepare` method of the `UIImpactFeedbackGenerator` is called to inform the system that haptic feedback is about to occur so that it can minimize latency.

The `ImpactOccurred` method then triggers haptic feedback.

<a name="UINotificationFeedbackGenerator"></a>

### UINotificationFeedbackGenerator

Use this feedback generator for notifications such as an action completing, failing or any other type of warning.

Use the following code to trigger notification feedback:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

A new instance of the `UINotificationFeedbackGenerator` class is created and its `Prepare` method is called to inform the system that haptic feedback is about to occur so that it can minimize latency.

The `NotificationOccurred` is called to trigger haptic feedback with a given `UINotificationFeedbackType` of:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator"></a>

### UISelectionFeedbackGenerator

Use this feedback generator for a selection actively changing such as picking an item from a list.

Use the following code to trigger selection feedback:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

A new instance of the `UISelectionFeedbackGenerator` class is created and its `Prepare` method is called to inform the system that haptic feedback is about to occur so that it can minimize latency.

The `SelectionChanged` method then triggers haptic feedback.

## Summary

This article has covered the new types of haptic feedback available in iOS 10 and how to implement them in Xamarin.iOS.

## Related Links

- [iOS 10 Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)