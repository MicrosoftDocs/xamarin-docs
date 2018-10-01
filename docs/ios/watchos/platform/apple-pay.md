---
title: "Apple Pay on watchOS in Xamarin"
description: "This article covers the enhancements Apple has made to Apple Pay in watchOS 3 and how to implement them in Xamarin.iOS for Apple Watch."
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
---

# Apple Pay on watchOS in Xamarin

Apple has made several enhancements to Apple Pay in watchOS 3 that adds support for In-App Payments. This allows the user to securely provide payment and contact information to pay for physical goods and services directly from the Apple Watch.


## About Apple Pay Enhancements

As Stated above, Apple has made several enhancements to Apple Pay in watchOS 3 that allow for secure payment and contact information to pay for physical goods and services directly from the Apple Watch. These enhancements are provided by modifications to the PassKit framework.

With iOS 10 and watchOS 3, several new APIs have been added that work with both iOS and watchOS to support dynamic payment networks and a new sandbox test environment.

## PassKit Framework Enhancements

In iOS 10, the PassKit framework has been expanded to support Apple Pay outside of `UIKit` and to allow card issuers to present their cards from within their apps. 

### Supporting Apple Pay Outside of UIKit

By using [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) and [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), an app can support the same functionality provided by [PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) without using UIKit. While this new API is required for supporting Apple Pay on the Apple Watch (and in specific Intents as well), it is optional in other situations (such as existing apps). However, Apple suggests moving to the new API as soon as possible to provide broad Apple Pay support throughout all of the developer's apps with a single code base. For more information about Intents and Siri integration, please see our [Introduction to SiriKit](~/ios/platform/sirikit/index.md) documentation.

### Presenting Issuer Cards from within Apps

With iOS 10 and watchOS 3, new features have been added to the PassKit framework that allow card issuers to present their payment cards from within their own apps. The developer can add a `PKPaymentButtonTypeInStore` UIButton to the app's user interface that will display an Apple Pay button for a card.

The `PresentPaymentPass` method of the [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) class can also be used to programmatically display the card.

## New Payment Network Support

New to iOS 10 and watchOS 3, an app can automatically support a new payment network when it becomes available without the developer having to modify, recompile the app and resubmit it to the App Store.

The new [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) method of the `PKPaymentNetwork` class allows an app to discover the networks available on the user's device at runtime. Additionally, the [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) property has been expanded to take the payment provider's name as an argument. Using these methods, an app can automatically support any network that the payment provider supports.

For more information, please see our [Apple Pay Configuration](~/ios/platform/apple-pay.md) and Apple's [Apple Pay Guide](https://developer.apple.com/apple-pay/).

## New Testing Environment

With iOS 10 and watchOS 3, Apple introduced a new testing environment that allows the developer to provision test payment cards directly on an iOS device. This new testing environment then returns encrypted test payment data to the app.

To enable the new testing environment, do the following:

1. Create a new testing iCloud Account in iTunes Connect.
2. Log into the iOS device with the new testing account.
3. Set the desired region to test the app in.
4. Use one of the test payment cards from the [Apple Pay Guide](https://developer.apple.com/apple-pay/) to make payments.

> [!NOTE]
> By switching iCloud Accounts, the device will automatically switch to the new testing environment. However, Apple still **requires** the app to be tested with real cards in a production environment before submission to the iTunes App Store.

## Summary

This article has covered the enhancements Apple has made to Apple Pay in watchOS 3 and how to implement them in Xamarin.iOS.
