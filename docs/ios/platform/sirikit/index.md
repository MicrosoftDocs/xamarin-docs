---
title: "SiriKit in Xamarin.iOS"
description: "This article shows how to use SiriKit in a Xamarin.iOS app to provide services that are accessible to the user using Siri on an iOS device."
ms.service: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# SiriKit in Xamarin.iOS

_This article shows how to use SiriKit in a Xamarin.iOS app to provide services that are accessible to the user using Siri on an iOS device._

New to iOS 10, SiriKit allows an iOS app to provide services that are accessible to the user using Siri and the Maps app on an iOS device using App Extensions and the new **Intents** and **Intents UI** frameworks.

Siri works with the concept of **Domains**, groups of know actions for related tasks. Each interaction that an app has with Siri must fall into one of its known service Domains as follows:

- Audio or video calling.
- Booking a ride.
- Managing workouts.
- Messaging.
- Searching photos.
- Sending or receiving payments.

When the user makes a request of Siri involving one of an App Extension's services, SiriKit sends the extension an **Intent** object that describes the user's request along with any supporting data. The App Extension then generates the appropriate **Response** object for the given **Intent**, detailing how the extension can handle the request.

## [Understanding SiriKit Concepts](~/ios/platform/sirikit/understanding-sirikit.md)

This article covers the key concepts that will be required for working with SiriKit in a Xamarin.iOS app. It covers the new Intents and Intents UI Extension Points and how they work with App and User Vocabulary to open an app to Siri.

## [Implementing SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

This article covers the steps required to implement SiriKit support in a Xamarin.iOS apps. The developer should read the Understanding SiriKit Concepts guide above before attempting to add SiriKit support to an app, as key concepts are covered that will be required for successful implementation.

## Related Links

- [ElizaChat Sample](/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Intents Framework Reference](https://developer.apple.com/reference/intents)
- [Intents UI Framework Reference](https://developer.apple.com/reference/intentsui)