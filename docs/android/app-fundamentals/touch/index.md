---
title: "Touch and Gestures in Xamarin.Android"
description: "Touch screens on many of today's devices allow users to quickly and efficiently interact with devices in a natural and intuitive way. This interaction is not limited just to simple touch detection - it is possible to use gestures as well. For example, the pinch-to-zoom gesture is a very common example of this by pinching a part of the screen with two fingers the user can zoom in or out. This guide examines touch and gestures in Android."
ms.service: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Touch and Gestures in Xamarin.Android

_Touch screens on many of today's devices allow users to quickly and efficiently interact with devices in a natural and intuitive way. This interaction is not limited just to simple touch detection - it is possible to use gestures as well. For example, the pinch-to-zoom gesture is a very common example of this by pinching a part of the screen with two fingers the user can zoom in or out. This guide examines touch and gestures in Android._

## Touch Overview

iOS and Android are similar in the ways they handle touch. Both can support multi-touch - many points of contact on the screen - and complex gestures. This guide introduces some of the similarities in concepts, as well as the particularities of implementing touch and gestures on both platforms.

Android uses a `MotionEvent` object to encapsulate touch data, and methods on the View object to listen for touches.

In addition to capturing touch data, both iOS and Android provide means for interpreting patterns of touches into gestures. These gesture recognizers can in turn be used to interpret application-specific commands, such as a rotation of an image or a turn of a page. Android provides a handful of supported gestures, as well as resources to make adding complex custom gestures easy.

Whether you are working on Android or iOS, the choice between touches and gesture recognizers can be a confusing one. This guide recommends that in general, preference should be given to gesture recognizers. Gesture recognizers are implemented as discrete classes, which provide greater separation of concerns and better encapsulation. This makes it easy to share the logic between different views, minimizing the amount of code written.

This guide follows a similar format for each operating system: first, the platform’s touch APIs are introduced and explained, as they are the foundation on which touch interactions are built. Then, we dive into the world of gesture recognizers – first by exploring some common gestures, and finishing up with creating custom gestures for applications. Finally, you'll see how to track individual fingers using low-level touch tracking to create a finger-paint program.

## Sections

- [Touch in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Walkthrough: Using Touch in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Multi-Touch tracking](touch-tracking.md)

## Summary

In this guide we examined touch in Android. For both operating systems, we learned how to enable touch and how to respond to the touch events. Next, we learned about gestures and some of the gesture recognizers that both Android and iOS provide to handle some of the more common scenarios. We examined how to create custom gestures and implement them in applications. A walkthrough demonstrated the concepts and APIs for each operating system in action, and you also saw how to track individual fingers.

## Related Links

- [Android Touch Start (sample)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android Touch Final (sample)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
- [FingerPaint (sample)](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)