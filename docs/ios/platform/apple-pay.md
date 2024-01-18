---
title: "Apple Pay in Xamarin.iOS"
description: "This guide explores setting up the Xamarin.iOS environment for use with Apple Pay to pay for physical goods, such as food, entertainment, and memberships via your app. It includes information on the required identifiers, certificates and entitlements."
ms.service: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
no-loc: [Objective-C]
---

# Apple Pay in Xamarin.iOS

_This guide explores setting up the Xamarin.iOS environment for use with Apple Pay to pay for physical goods, such as food, entertainment, and memberships via your app. It includes information on the required identifiers, certificates and entitlements._

Apple Pay was introduced alongside iOS 8, enabling users to pay for physical goods such as food,
entertainment, and memberships via their iOS devices. It is available on iPhone 6 and iPhone 6 Plus, and can also be paired with the Apple Watch for in-store purchases. When used on an iPhone, it uses Touch ID as a way to confirm and
authorize transactions to a user's credit or debit card.

## Requirements

Apple Pay is only available within iOS 8 and above, and therefore requires a minimum of Xcode 6.

The following items are also required to integrate Apple Pay into your app:

- Payment Processor Platform
- Merchant Identifier
- An Apple Pay certificate
- Apple Pay entitlement

This document will look at these items in more detail.

## Differences between Apple Pay and IAP

The primary difference between Apple Pay and *In-App Purchasing* (IAP), pertains to the products that
they sell. *Physical* goods are sold via Apple Pay; food, accommodation and physical entertainment
(such as cinema tickets) are all examples of this. In contrast, IAP sells *virtual* goods, such as premium or extra content, and subscriptions–think additional months of a streaming service, or extra lives in a game.

The frameworks used are also a key difference;
[PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) is used for Apple Pay, while
[StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) provides the framework API for IAP.

With Apple Pay, Apple [states](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) that it “[Does] not charge users, merchants or developers to use Apple Pay
for Payments”. In comparison, IAP has a 30% charge for each transaction. Moreover, with Apple Pay, the
transaction does not go through Apple at all, instead, it goes through a Payment platform.

## Using a Payment Processor Platform

One of the fundamental parts of Apple Pay is the processing of payments. While it
is possible to do this yourself, it requires significant knowledge of
cryptography - as detailed in Apple’s [Payment Processing guide](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Payment processing platforms, on the other hand, handle these operations for you, allowing you
to concentrate on building your app.

Two options include:

- **Stripe** - sign up at [Stripe.com](https://stripe.com/) to access their APIs.

- **JudoPay** - check out their [Xamarin sample code on github](https://github.com/Judopay/Xamarin-Sample-App), and register at
[JudoPay.com](https://www.judopay.com/).

## Provisioning for Apple Pay

Configuring an app to use Apple Pay requires setup on the Apple Developer Portal and within your app. There are a number of steps that should be followed to successfully provision your app for Apple pay:

1. Create a Merchant ID:
    - Follow the steps [here](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Create an App ID with the Apply Pay capability and add the merchant to it:
    - Follow the steps [here](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Generate a certificate for the Merchant ID:
    - Follow the steps [here](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generate a Provisioning Profile with the newly created App ID:
    - Follow the steps [here](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Add Apple Pay entitlements:
    - Select the Apple pay entitlement as detailed [here](~/ios/deploy-test/provisioning/entitlements.md), or manually add the key/value pair to the file from [here](~/ios/deploy-test/provisioning/entitlements.md)

## Working with Apple Pay

Apple has made several enhancements to Apple Pay in iOS 10 that allow the user to make secure payments from websites and through interaction with Siri and Maps.

With iOS 10, several new APIs have been added that work with both iOS and watchOS to support dynamic payment networks and a new sandbox test environment.

### Apple Pay Website Integration

New to iOS 10, the developer can incorporate Apple Pay directly into their websites using **ApplePay JS**. Users browsing the website with Safari in iOS or macOS can make payments with Apple Pay by validating the transaction on their iPhone or Apple Watch. For more information, please see Apple's [ApplePay JP Framework Reference](https://developer.apple.com/reference/applepayjs).

### PassKit Framework Enhancements

In iOS 10, the PassKit framework has been expanded to support Apple Pay outside of `UIKit` and to allow card issuers to present their own cards from within their apps.

#### Supporting Apple Pay Outside of UIKit

By using [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) and [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), an app can support the same functionality provided by [PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) without using UIKit. While this new API is required for supporting Apple Pay on the Apple Watch (and in specific Intents as well), it is optional in other situations (such as existing apps). However, Apple suggests moving to the new API as soon as possible to provide broad Apple Pay support throughout all of the developer's apps with a single code base. For more information about Intents and Siri integration, please see our [Introduction to SiriKit](~/ios/platform/sirikit/index.md) documentation.

#### Presenting Issuer Cards from within Apps

With iOS 10, new features have been added to the PassKit framework that allow card issuers to present their cards from within their own apps. The developer can add a `PKPaymentButtonTypeInStore` UIButton to the app's user interface that will display an Apple Pay button for a card.

The `PresentPaymentPass` method of the [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) class can also be used to programmatically display the card.

### New Payment Network Support

New to iOS 10, an app can automatically support a new payment network when it becomes available without the developer having to modify, recompile the app and resubmit it to the App Store.

The new [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) method of the `PKPaymentNetwork` class allows an app to discover the networks available on the user's device at runtime. Additionally, the [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) property has been expanded to take the payment provider's name as an argument. Using these methods, an app can automatically support any network that the payment provider supports.

For more information, please see our [Apple Pay Configuration](~/ios/platform/apple-pay.md) and Apple's [Apple Pay Guide](https://developer.apple.com/apple-pay/).

### New Testing Environment

With iOS 10, Apple introduced a new testing environment that allows the developer to provision test payment cards directly on an iOS device. This new testing environment then returns encrypted test payment data to the app.

To enable the new testing environment, do the following:

1. Create a new testing iCloud Account in iTunes Connect.
2. Log into the iOS device with the new testing account.
3. Set the desired region to test the app in.
4. Use one of the test payment cards from the [Apple Pay Guide](https://developer.apple.com/apple-pay/) to make payments.

> [!IMPORTANT]
> By switching iCloud Accounts, the device will automatically switch to the new testing environment. However, Apple still **requires** the app to be tested with real cards in a production environment before submission to the iTunes App Store.

## Summary

In this article, we explored the different items required to use Apple Pay within your app. We
looked at how to create a Merchant ID, and how it is used within the **Entitlements.plist**, which needs to be modified manually.

## Related Links

- [In-App Purchasing](~/ios/platform/in-app-purchasing/index.md)
- [Intro to PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (sample)](/samples/xamarin/ios-samples/ios9-emporium)