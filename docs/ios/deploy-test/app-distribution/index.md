---
title: "Xamarin.iOS App Distribution Overview"
description: "This document gives an overview of the distribution techniques that are available for Xamarin.iOS applications and serves as a pointer to more detailed documents on the topic."
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
---

# Xamarin.iOS App Distribution Overview

_This document gives an overview of the distribution techniques that are available for Xamarin.iOS applications and serves as a pointer to more detailed documents on the topic._

Once an Xamarin.iOS app has been developed, the next step in the software development lifecycle is to distribute the app to users, as shown in the highlighted section of the diagram below:


[![](images/publishingdiagram.png "After the iOS app has been developed, the next step is to distribute the app to users, as shown in the highlighted section of this diagram")](images/publishingdiagram.png#lightbox)


Apple provides the following ways to distribute an iOS application, which are supported by Xamarin.iOS:

1. [**The App Store**](#App_Store_Distribution)
2. [**In-House (Enterprise)**](#In-House_Distribution)
2. [**Ad Hoc**](#Ad_Hoc_Distribution)

All these scenarios require that applications be provisioned using the appropriate *provisioning profile*. Provisioning profiles are files that contain code signing information, as well as the identity of the application and the intended distribution mechanism. For the non-App Store distribution they also contain information about what devices the app can be deployed to.

<a name="App_Store_Distribution"/>

## App Store Distribution

> [!IMPORTANT]
> Apple [has indicated](https://developer.apple.com/news/?id=05072018a) that
> starting in July 2018, all apps and updates submitted to the App Store
> must have been built with the iOS 11 SDK and 
> [support the iPhone X display](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

This is the main way that iOS applications are distributed to consumers on iOS devices. All apps submitted to the App Store require approval by Apple.

Apps are submitted to the App Store through a portal called *iTunes Connect*. The [Configure your App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide provides more information on how to set up and use this portal to prepare a Xamarin.iOS app for publishing in the App Store.

It is important to note that only developers who belong to the **Apple Developer Program** have access to iTunes Connect. Members of the **Apple Developer Enterprise Program** do not have access.

For more information, please visit the [App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) guide.

<a name="In-House_Distribution"/>

## In-House Distribution

Sometimes called *Enterprise Distribution*, in-house distribution allows members of the **Apple Developer Enterprise Program** to distribute apps internally to other members of the same organization. In-house distribution has the advantages of not requiring an App Store review, and having no limit on the number of devices on which an application can be installed. However, it is important to note that **Apple Developer Enterprise Program** members do **not** have access to iTunes Connect, and therefore the licensee is responsible for distributing the app.

For more information on getting set-up and how to distribute an application In-House, please refer to the [In-House Distribution guide](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## Ad Hoc Distribution

Xamarin.iOS applications can be user-tested via ad hoc distribution, which is available on both the **Apple Developer Program**, and the **Apple Developer Enterprise Program**, and allows up to 100 iOS devices to be tested. The best use case for ad hoc distribution is distribution within a company when iTunes Connect is not an option.

For more information on getting set-up and how to distribute an application In-House, please refer to the [Ad Hoc Distribution guide](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## Summary

This article gave a brief overview of the distribution mechanisms that are available for Xamarin.iOS applications. It introduced the iTunes App Store, Ad Hoc and In-house deployment, and provided links to more detailed information.

## Related Links

- [App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Configuring an App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publishing to the App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [In-House Distribution](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad Hoc Distribution](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [The iTunesMetadata.plist File](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA Support](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Troubleshooting](~/ios/deploy-test/troubleshooting.md)
