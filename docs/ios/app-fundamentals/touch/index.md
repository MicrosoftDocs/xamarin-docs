---
title: "Touch"
description: "Touch screens on many of today’s devices allow users to quickly and efficiently interact with devices in a natural and intuitive way. This interaction is not limited just to simple touch detection – it is possible to use gestures as well. For example, the pinch-to-zoom gesture is a very common example of this – by pinching a part of the screen with two fingers the user can zoom in or out. This guide examines touch and gestures in iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A17FD28-313F-4AAC-B82B-3847B4D64A88
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
---

# Touch

_Touch screens on many of today’s devices allow users to quickly and efficiently interact with devices in a natural and intuitive way. This interaction is not limited just to simple touch detection – it is possible to use gestures as well. For example, the pinch-to-zoom gesture is a very common example of this – by pinching a part of the screen with two fingers the user can zoom in or out. This guide examines touch and gestures in iOS._


Like other mobile platforms, iOS has a number of ways to handle touch. It can support multi-touch — many points of contact on the screen — and complex gestures. This guide introduces some of the concepts, as well as the particularities of implementing touch and gestures on iOS.

iOS encapsulates touch data in the `UITouch` class, which is made available to applications through a series of `UIResponder` methods. Applications can override these methods in subclasses of `UIView` and `UIViewController`, both of which inherit from `UIResponder`.

In addition to capturing touch data, iOS provides means for interpreting patterns of touches into gestures. These gesture recognizers can in turn be used to interpret application-specific commands, such as a rotation of an image or a turn of a page. iOS provides a rich collection of classes to handle common gestures with minimum added code.

The choice between touches and gesture recognizers can be a confusing one. This guide recommends that in general, preference should be given to gesture recognizers. Gesture recognizers are implemented as discrete classes, which provide greater separation of concerns and better encapsulation. This makes it straightforward to share the logic between different views, minimizing the amount of code written.

However, there are times when you need to use low-level touch processing and even track multiple fingers, for example, to create a finger-paint program.

## Sections

-  [Touch in iOS](touch-in-ios.md)
-  [Walkthrough: Using Touch in iOS](ios-touch-walkthrough.md)
-  [Multi-Touch Tracking](touch-tracking.md)

This guide serves as introduction to Touch in iOS. For more information on using 3D Touch and Haptic Feedback in iOS, which were introduced in iOS 9 and 10 respectively please refer to the specific guides below:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Providing Haptic Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)



## Related Links

- [iOS Touch Start (sample)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch Final (sample)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (sample)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
