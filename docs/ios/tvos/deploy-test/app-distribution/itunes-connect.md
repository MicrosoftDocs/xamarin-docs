---
title: "Configure your tvOS App in iTunes Connect"
description: "This article provides a supplemental guide to the iOS Configure your App in iTunes Connect for the tvOS specific configurations."
ms.service: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# Configure your tvOS App in iTunes Connect

_This article provides a supplemental guide to the iOS Configure your App in iTunes Connect for the tvOS specific configurations._

In addition to the configurations and setting that you will need to make by following the iOS [Configure your App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide, this document covers the specific configurations that will be required to release a Xamarin.tvOS app in the Apple TV App Store.

<a name="Adding-a-tvOS-Release-Version"></a>

## Adding a tvOS Release Version

Whether you are creating a new App to be released on the Apple TV App Store, or adding Apple TV support to an existing iOS app, you'll need to have created an iTunes Connect Record and configured it using the following iOS specific guides:

- [Creating an iTunes Connect Record](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Managing App Videos and Screenshots](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Managing Name, Description, What's New, Keywords and URLs](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Maintaining General Information](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Optionally, you might also require:

- [Maintaining Game Center Information](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Maintaining In-App Purchase Information](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

With all of the above steps completed, open your app's iTunes Connect Record and select to add tvOS support using the left hand sidebar:

[![Add tvOS support using the left hand sidebar](itunes-connect-images/connect01.png)](itunes-connect-images/connect01.png#lightbox)

The tvOS specific information screens will then be available for the given iTunes Connect Record:

[![The tvOS specific information screen](itunes-connect-images/connect02.png)](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information"></a>

## tvOS Version Information

From the left hand sidebar, select **1.0 Prepare for Submission** under the tvOS APP section:

[![tvOS Version Information](itunes-connect-images/connect03.png)](itunes-connect-images/connect03.png#lightbox)

On this screen provide the following information:

- The required Screenshots, Description, keywords and URLs.
- General App Information such as Version Number, Copyright and Age Rating.
- Optional In-App Purchases.
- Optional Game Center support with Leaderboards and Achievements.
- Required App Review Information such as Contact, Demo Accounts and Notes.

Once you have entered the required information, click the **Save** button in the upper right-hand corner of the screen to save your changes:

[![tvOS Version Information ready for submission](itunes-connect-images/connect04.png)](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review"></a>

## Preparing to Submit for Review

When you are finally ready to submit your Xamarin.tvOS app to the Apple TV App Store for review, return to the app's iTunes Connect Record and click the **Submit for Review** button in the upper right hand corner of the screen:

[![Submit for Review](itunes-connect-images/connect05.png)](itunes-connect-images/connect05.png#lightbox)

<a name="Summary"></a>

## Summary

This article gave overview of the tvOS specific setting required in iTunes Connect to release a tvOS app to the Apple TV App Store.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)