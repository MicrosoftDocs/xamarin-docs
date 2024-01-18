---
title: "In-App Purchasing in Xamarin.iOS"
description: "This document describes how to sell digital products and services using the StoreKit APIs. It links to guides that discuss configuration, consumable products, non-consumable products, transactions, subscriptions, and more."
ms.service: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
no-loc: [Objective-C]
---

# In-App Purchasing in Xamarin.iOS

iOS applications can sell digital products or services using StoreKit – a
set of APIs provided by iOS that communicate with Apple’s servers to conduct
financial transactions with the user via their Apple ID. The StoreKit APIs are
primarily concerned with retrieving product information and conducting
transactions – there is no user-interface component. Applications that
implement in-app purchasing must build their own user interface and track
purchased items with custom code to provide the required products or services to
the user.

Providing in-app purchase functionality requires a
number of steps:

- **Configuring your app** –The application’s provisioning profile must be setup correctly.
- **Creating products** – Product descriptions and prices must be created in the iTunes Connect portal.
- **Implementing StoreKit** – The StoreKit API must be implemented according to the types of products being sold.
- **Building the user interface and the products themselves** – The products must be implemented, including mechanisms to track each purchase and backup/restore them if appropriate.
- **Monitoring sales and receiving funds** – Use information provided by iTunes Connect to monitor sales trends and track your income.

This document explains how to complete all these steps to provide
in-app purchases using Xamarin.iOS.

## Requirements

To support In-App Purchasing you must use Xamarin.iOS 5.0 or newer with Xcode 7 and above.

## Contents

- [In-App Purchase Basics and Configuration](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

- [StoreKit Overview and Retrieving Product Information](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

- [Purchasing Consumable Products](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

- [Purchasing Non-Consumable Products](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

- [Transactions and Verification](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

- [Subscriptions and Reporting](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## Summary

This article has introduced the concept of in-app purchasing, outlined how to
configure your application to take advantage of it and presented examples using
Xamarin.iOS. It has covered:

- **iOS Provisioning Portal** – Guidelines for enabling in-app purchase functionality.
- **iTunes Connect** – Configuring products to sell in your app.
- **Store Kit** – Explanation of the classes used to build in-app purchase features.
- **Coding your app for purchasing** – Examples of how to build in-app purchase into a Xamarin.iOS app.
- **Reporting** – Overview of the statistics available via iTunes Connect.

## Related Links

- [InAppPurchaseSample](/samples/xamarin/ios-samples/storekit/)
- [In App Purchase Programming Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect Developer Guide](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Store Kit Framework Reference](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [In-App Purchase Product Identifiers Q&A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [In-App Purchase Technical Note](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Your First App Store Submission](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store Submission Tips](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Managing Your Apps](https://developer.apple.com/appstore/resources/managing/index.html)