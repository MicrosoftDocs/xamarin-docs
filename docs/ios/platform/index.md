---
title: "iOS platform features overview"
description: "This document links to various guides that describe features introduced in various versions of iOS, and other iOS platform features."
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
---
# iOS platform features overview

This page lists recent iOS releases as well as highlighting some of the Apple frameworks you can access with Xamarin.iOS.

## iOS releases

| Release | Description |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Introduction to iOS 14](~/ios/platform/ios14/index.md) | This document describes Xamarin.iOS 14.|
| [Introduction to iOS 13](~/ios/platform/ios13/index.md) | This document describes Xamarin.iOS 13.|
| [Introduction to iOS 12](~/ios/platform/introduction-to-ios12/index.md) | This document describes iOS 12 features available for use when building Xamarin.iOS applications.|
| [Introduction to iOS 11](~/ios/platform/introduction-to-ios11/index.md) | This document describes the new and updated features in iOS 11 and Xcode 9,such as ARKit, Core ML, Core NFC, Drag and Drop, MapKit, PDFKit, SiriKit,and Vision. It links to guides that describe how to use these features with Xamarin.iOS. |
| [Introduction to iOS 10](~/ios/platform/introduction-to-ios10/index.md) | iOS 10 includes several new APIs and services that allow you to develop apps with new features and functionality. With iOS 10, apps have new abilities such as extending Maps, Messages, Phone and Siri. This section shows hows to take advantage of these features in a Xamarin.iOS app. |
| [Introduction to iOS 9](~/ios/platform/introduction-to-ios9/index.md)   | This section defines the changes made in iOS 9 when upgrading from iOS 8 and how to use these features in a Xamarin.iOS app. |
| [Introduction to iOS 8](~/ios/platform/introduction-to-ios8.md)         | iOS 8 made a large number of changes to the operating system from iOS 7. Here, we show what they are and how to use them. |
| [Introduction to iOS 7](~/ios/platform/introduction-to-ios7/index.md)   | About the major new APIs introduced in iOS 7, including View Controller transitions, enhancements to UIView animations, UIKit Dynamics, and Text Kit. |
| [Introduction to iOS 6](~/ios/platform/introduction-to-ios6/index.md)   | Explanations of the features introduced in iOS 6, including Collection Views, Pass Kit, Event Kit, and the Social Framework. |

## [Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay was introduced alongside iOS 8, enabling users to pay for physical goods such as food, entertainment, and memberships via their iOS devices. It is available on iPhone 6 and iPhone 6 Plus, and can also be paired with the Apple Watch for in-store purchases. When used on an iPhone, it uses Touch ID as a way to confirm and authorize transactions to a user's credit or debit card.

## [CallKit](~/ios/platform/callkit.md)

The new CallKit API in iOS 10 provides a way for VOIP apps to integrate with the iPhone UI and provide a familiar interface and experience to the end user. With this API users can view and interact with VOIP calls from the iOS device's Lock Screen and manage contacts using the Phone app's **Favorites** and **Recents** views.

## [Contacts and ContactsUI](~/ios/platform/contacts.md)

With the introduction of iOS 9, Apple has released two new frameworks, `Contacts` and `ContactsUI`, that replace the existing Address Book and Address Book UI frameworks used by iOS 8 and earlier.

## [Document Picker](~/ios/platform/document-picker.md)

The Document Picker allows documents to be shared between apps. These documents may be stored in iCloud or in a different app’s directory. Documents are shared via the set of [Document Provider Extensions](~/ios/platform/extensions.md) the user has installed on their device.

## [EventKit](~/ios/platform/eventkit.md)

iOS has two calendar-related applications built-in: the Calendar Application, and the Reminders Application. It’s straightforward enough to understand how the Calendar Application manages calendar data, but the Reminders Application is less obvious. Reminders can actually have dates associated with them in terms of when they’re due, when they’re completed, etc. As such, iOS stores all calendar data, whether it be calendar events or reminders, in one location, called the *Calendar Database*.

## [iOS extensions](~/ios/platform/extensions.md)

Extensions, as introduced in iOS 8, are specialized `UIViewControllers` that are presented by iOS inside standard contexts such as within the **Notification Center**, as custom keyboard types requested by the user to perform specialized input or other contexts like editing a photo where the Extension can provide special effect filters.

## [Graphics and animation in iOS](~/ios/platform/graphics-animation-ios/index.md)

Graphics and Animation in iOS covers core graphics concepts in iOS such as CoreImage, Core Graphics and Core Animation.

## [Handoff](~/ios/platform/handoff.md)

Apple introduced Handoff in iOS 8 and OS X Yosemite (10.10) to provide a common mechanism for the user to transfer activities started on one of their devices, to another device running the same app or another app that supports the same activity.

## [HealthKit](~/ios/platform/healthkit.md)

Health Kit provides a secure datastore for the user’s health-related information. Health Kit apps may, with the user’s explicit permission, read and write to this datastore and receive notifications when pertinent data is added. Apps can present the data, or user’s can use the Apple's provided Health app to view a dashboard of all their data.

## [HomeKit](~/ios/platform/homekit.md)

Apple introduced HomeKit in iOS 8 to provide a common framework for discovering and communicating with home automation devices in a user's home. HomeKit provides a common platform for configuring devices and setting up actions to control them.

## [In-app purchasing](~/ios/platform/in-app-purchasing/index.md)

iOS applications can sell digital products or services using StoreKit – a
set of APIs provided by iOS that communicate with Apple’s servers to conduct financial transactions with the user via their Apple ID. The StoreKit APIs are primarily concerned with retrieving product information and conducting transactions – there is no user-interface component. Applications that implement in-app purchasing must build their own user interface and track purchased items with custom code to provide the required products or services to the user.

## [iOS gaming APIs](~/ios/platform/gaming/index.md)

Apple has made several technological improvements to the gaming APIs in iOS 9 that make it easier to implement game graphics and audio in a Xamarin.iOS app. These include both ease of development through high-level frameworks and harnessing the power of the iOS device's GPU for improved speed and graphic abilities.

## [Message app integration](~/ios/platform/message-app-integration/index.md)

New to iOS 10, a Message App Extension integrates with the **Messages** app and presents new functionality to the user. The extension can send text, stickers, media files and interactive messages.

## [Multitasking for iPad](~/ios/platform/multitasking.md)

iOS 9 adds multitasking support for running two apps at the same time on specific iPad hardware. Multitasking for iPad is supported via the following features: Slide Over, Split View & Picture in Picture.

## [PassKit](~/ios/platform/passkit.md)

Passbook is an app for iPhones and iPod touches with iOS 6. It stores and displays barcodes and other information to link customer transactions on their phone with the ‘real world’. Passes are generated by merchants and sent to the customer via email, URLs or from within a merchant’s own iOS app. Passbook stores and organizes all the Passes on a phone, and displays Pass reminders on the lock-screen depending on the date/time or the location of the device.

This document introduces Passbook, using the Pass Kit API with Xamarin.iOS, and discusses how to implement Passes on your server.

## [PhotoKit](~/ios/platform/photokit.md)

Photo Kit is a new framework that allows applications to query the system image library and create custom user interfaces to view and modify its contents. It includes a number of classes that represent image and video assets, as well as collections of assets such as albums and folders.

## [Request app review](~/ios/platform/request-app-review.md)

New to iOS 10.3, the `RequestReview()` method allows an iOS app to ask the user to rate or review it. When this method is called in a shipping app that the user has installed from the App Store, iOS 10 will handle the entire rating and review process for the developer. Because this process is governed by App Store policy, an alert may or may not be displayed.

## [Search APIs](~/ios/platform/search/index.md)

Search has been expanded in iOS 9 to provide great new ways to access information and features inside a Xamarin.iOS app. Using the new App Search APIs, app content is made searchable through Spotlight and Safari search results, Handoff and Siri Reminders and Suggestions. This allows users to quickly access activities and information deep within your app.

## [SiriKit](~/ios/platform/sirikit/index.md)

New to iOS 10, SiriKit allows an iOS app to provide services that are accessible to the user using Siri and the Maps app on an iOS device using App Extensions and the new **Intents** and **Intents UI** frameworks.

## [Social framework](~/ios/platform/social-framework.md)

The Social Framework provides a unified API for interacting with social networks including _Twitter_ and _Facebook_, as well as _SinaWeibo_ for users in China.

## [Speech recognition](~/ios/platform/speech.md)

iOS 10 includes a new Speech API that allows the app to support continuous speech recognition and transcribe speech (from live or recorded audio streams) into text.

## [TextKit](~/ios/platform/textkit.md)

Text Kit is a new API that offers powerful text layout and rendering features. It is built on top of the low level Core Text framework, but is much easier to use than Core Text.

## [3D Touch](~/ios/platform/3d-touch.md)

This article will provide and introduction to using the new 3D Touch APIs to add pressure sensitive gestures to your Xamarin.iOS apps that are running on the new iPhone 6s and iPhone 6s Plus devices.

## [Touch ID and Face ID with Xamarin.iOS](~/ios/platform/touch-id-face-id.md)

Touch ID and Face ID are biometric authentication systems available since iOS 8. This article and sample describe how to use Touch ID and Face ID with Xamarin.iOS.

## [User notifications](~/ios/platform/user-notifications/index.md)

New to iOS 10, the User Notification framework allows for the delivery and handling of local and remote notifications. Using this framework, the app or App Extension can schedule the delivery of local notifications by specifying a set of conditions such as location or time of day.

## [Wide Color](~/ios/platform/wide-color.md)

iOS 10 and macOS Sierra enhances the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

## [Binding Objective-C](binding-objective-c/index.md)

When working on iOS, you might encounter cases where you want to consume a
third-party Objective-C library. In those situations, you can use MonoTouch's
Binding Projects to create a C# binding to the native Objective-C libraries. The
project uses the same tools that we use to bring the iOS APIs to C#. This
document describes how to bind Objective-C APIs.

## [Bind iOS Swift Libraries](binding-swift/index.md)

This document describes how to create C# bindings to Swift code, making it possible to consume native libraries and CocoaPods in a Xamarin.iOS application.

## [Referencing native libraries](native-interop.md)

Xamarin.iOS supports linking with both native C libraries and Objective-C
libraries. This document discusses how to link your native C libraries with your
Xamarin.iOS project.

## [Embedded frameworks](embedded-frameworks.md)

Explains how to embed Objective-C user frameworks in Xamarin.iOS apps.
