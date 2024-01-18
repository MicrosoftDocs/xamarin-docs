---
title: "Backgrounding in Xamarin.iOS"
description: "Background processing or backgrounding is the process of letting applications perform tasks in the background while another application is running in the foreground. This guide serves as an introduction to background processing in iOS."
ms.service: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
no-loc: [Objective-C]
---

# Backgrounding in Xamarin.iOS

_Background processing or backgrounding is the process of letting applications perform tasks in the background while another application is running in the foreground. This guide serves as an introduction to background processing in iOS._

Backgrounding in mobile applications is fundamentally different than the traditional concept of multitasking on the desktop. Desktop machines have a variety of resources available to an application, including screen real estate, power, and memory. Applications are able to run side-by-side and remain performant and usable. On a mobile device, resources are much more limited. It is difficult to show more than one application on a small screen, and running several applications at full speed would drain the battery. Backgrounding is a constant compromise between giving applications the resources to run the background tasks they require to perform well, and keeping the foregrounded application and the device responsive. Both iOS and Android have provisions for backgrounding, but they handle it in very different ways.

In iOS, backgrounding is recognized as an application state, and apps are moved in and out of the background state depending on the behavior of the app and the user. iOS also offers several options for wiring an app to run in the background, including asking the OS for time to complete an important task, operating as a type of known background-necessary application, and refreshing an application's content at designated intervals.

In this guide and accompanying walkthroughs, we are going to learn to perform application tasks in the background. We will cover key concepts and best practices, and then step through creating a real world app that receives location updates in the background.

## Contents

1. [Introduction to Backgrounding in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1. [Application Lifecycle Demo](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1. [iOS Backgrounding Techniques](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1. [Walkthroughs: Backgrounding in iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1. [iOS Backgrounding Guidance](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## Summary

In this guide, we introduced the different ways of doing background processing in iOS. We covered iOS Application States and examined the role backgrounding plays in the iOS Application Lifecycle. Additionally, we learned how we could register individual tasks or entire applications to operate in the background in iOS. Finally, we reinforced our understanding of backgrounding on iOS by building applications that perform updates in the background.

## Related Links

- [Backgrounding on Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (sample)](/samples/xamarin/ios-samples/lifecycledemo)
- [Location (sample)](/samples/xamarin/ios-samples/location)
- [Simple Background Transfer (sample)](/samples/xamarin/ios-samples/simplebackgroundtransfer)
- [iOS Background Execution](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)