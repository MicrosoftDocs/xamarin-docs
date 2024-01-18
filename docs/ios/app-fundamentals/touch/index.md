---
title: "Handling Touch in Xamarin.iOS Apps"
description: "This document links to guides that describe how to work with touch, multi-touch, gestures, and 3D Touch in a Xamarin.iOS app."
ms.service: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/23/2017
no-loc: [Objective-C]
---

# Handling Touch in Xamarin.iOS Apps

Like other mobile platforms, iOS has a number of ways to handle touch. It can support multi-touch — many points of contact on the screen — and complex gestures. This guide introduces some of the concepts, as well as the particularities of implementing touch and gestures on iOS.

iOS encapsulates touch data in the `UITouch` class, which is made available to applications through a series of `UIResponder` methods. Applications can override these methods in subclasses of `UIView` and `UIViewController`, both of which inherit from `UIResponder`.

In addition to capturing touch data, iOS provides means for interpreting patterns of touches into gestures. These gesture recognizers can in turn be used to interpret application-specific commands, such as a rotation of an image or a turn of a page. iOS provides a rich collection of classes to handle common gestures with minimum added code.

The choice between touches and gesture recognizers can be a confusing one. This guide recommends that in general, preference should be given to gesture recognizers. Gesture recognizers are implemented as discrete classes, which provide greater separation of concerns and better encapsulation. This makes it straightforward to share the logic between different views, minimizing the amount of code written.

However, there are times when you need to use low-level touch processing and even track multiple fingers, for example, to create a finger-paint program.

## Sections

- [Touch in iOS](touch-in-ios.md)
- [Walkthrough: Using Touch in iOS](ios-touch-walkthrough.md)
- [Multi-Touch Tracking](touch-tracking.md)

This guide serves as introduction to Touch in iOS. For more information on using 3D Touch and Haptic Feedback in iOS, which were introduced in iOS 9 and 10 respectively please refer to the specific guides below:

- [3D Touch](~/ios/platform/3d-touch.md)
- [Providing Haptic Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

## Related Links

- [iOS Touch Final (sample)](/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
- [FingerPaint (sample)](/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)