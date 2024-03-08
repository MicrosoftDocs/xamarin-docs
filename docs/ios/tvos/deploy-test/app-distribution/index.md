---
title: "tvOS App Distribution Overview"
description: "This document gives an overview of the distribution techniques that are available for Xamarin.tvOS app and serves as a pointer to more detailed documents on the topic."
ms.service: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# tvOS App Distribution Overview

_This document gives an overview of the distribution techniques that are available for Xamarin.tvOS app and serves as a pointer to more detailed documents on the topic._

Once your Xamarin.tvOS app has been developed, the next step in the software development lifecycle is to distribute your app to users, as shown in the highlighted section of the diagram below:

[![The software development lifecycle overview](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)

Apple provides the following ways to distribute a tvOS app, which are supported by Xamarin.tvOS:

1. [**The App Store**](#Apple-TV-App-Store-Distribution)
2. [**In-House (Enterprise)**](#In-House-Distribution) 
3. [**Ad Hoc**](#Ad_Hoc_Distribution) 

All these scenarios require that applications be provisioned using the appropriate *provisioning profile*. Provisioning profiles are files that contain code signing information, as well as the identity of the application and the intended distribution mechanism. For the non-App Store distribution they also contain information about what devices the app can be deployed to.

<a name="Apple-TV-App-Store-Distribution"></a>

## Apple TV App Store Distribution

This is the main way that tvOS apps are distributed to consumers on Apple TV devices. All apps submitted to the Apple TV App Store require approval by Apple.

Apps are submitted to the App Store through a portal called *iTunes Connect*. Please see our [Configure your App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide for information on the following topics:

- Managing Agreements, Tax and Banking.
- Creating an iTunes Connect Record.
- Managing App Videos and Screenshots.
- Managing App Name, Description, What's New, Keywords and URLs.
- Maintaining General App Information.
- Maintaining Game Center Information.
- Maintaining App Review Information.
- Maintaining Pricing Information.
- Maintaining In-App Purchase Information.
- Viewing Application Reviews.

While not written specifically for tvOS, this document provides information on how to set up and use this portal to prepare your app for publishing in the Apple TV App Store. Many of the same steps are required whether you are setting up an iOS or tvOS app.

Once you have completed all of the steps listed above, see our [Configure your tvOS App in iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) to add the tvOS app specific information that will be required.

It is important to note that only developers who belong to the **Apple Developer Program** have access to iTunes Connect. Members of the **Apple Developer Enterprise Program** do not have access.

If you are having issues submitting your Xamarin.tvOS app to the Apple TV App Store, please see our [Troubleshooting](~/ios/tvos/troubleshooting.md) guide. It contains several known issues that you might encounter and how to solve them in the Xamarin.tvOS.

For more information, please visit the [Publishing to the Apple TV App Store](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) guide.

<a name="In-House-Distribution"></a>

## In-House Distribution

Sometimes called *Enterprise Distribution*, in-house distribution allows members of the **Apple Developer Enterprise Program** to distribute apps internally to other members of the same organization. In-house distribution has the advantages of not requiring an App Store review, and having no limit on the number of devices on which an application can be installed. However, it is important to note that **Apple Developer Enterprise Program** members do **not** have access to iTunes Connect, and therefore the licensee is responsible for distributing the app.

For more information on getting set-up and how to distribute your application In-House, please refer to the [In-House Distribution guide](~/ios/deploy-test/app-distribution/in-house-distribution.md). This document is specific to iOS but the same techniques are used for tvOS apps.

<a name="Ad_Hoc_Distribution"></a>

## Ad Hoc Distribution

Xamarin.tvOS apps can be user-tested via ad hoc distribution, which is available on both the **Apple Developer Program**, and the **Apple Developer Enterprise Program**, and allows up to 100 Apple TV devices to be tested. The best use case for ad hoc distribution is distribution within your company when iTunes Connect is not an option.

For more information on getting set-up and how to distribute your app In-House, please refer to the [Ad Hoc Distribution guide](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Again, this document is specific to iOS but the same techniques are used for tvOS apps.

<a name="Summary"></a>

## Summary

This article gave a brief overview of the distribution mechanisms that are available for Xamarin.tvOS apps. It introduced the Apple TV App Store, Ad Hoc and In-house deployment, and provided links to more detailed information.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)