---
title: "tvOS Resources and Data Storage in Xamarin"
description: "This article describes how to work with resource and persistent data storage in a tvOS app built with Xamarin. It discusses iCloud data storage and on-demand resources."
ms.service: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# tvOS Resources and Data Storage in Xamarin

_This article covers working with resources and persistent data storage in a Xamarin.tvOS app._

<a name="tvOS-Resource-Limitations"></a>

## tvOS Resource Limitations

Unlike iOS devices, the new Apple TV provides extremely limited persistent, local storage for tvOS apps or data. For very small items (such as user preferences), your tvOS app still has access to `NSUserDefaults` with a [limit of 500 KB of data](https://forums.developer.apple.com/forums/thread/16967?answerId=50696022#50696022). However, if your Xamarin.tvOS app needs to persist larger amounts of information, it must store and retrieve that data from [iCloud](#iCloud-Data-Storage).

Additionally, tvOS limits the size of an Apple TV app to 200MB. If your app requires resources beyond this size, they will need to be packaged and loaded using [On-Demand Resources](#On-Demand-Resources) (up to an additional 2GB). Given these limitations, it is critical that you correctly time the downloading of additional assets to provide the best experience for your app's users. For more information, please see Apple's [On-Demand Resources Guide](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads"></a>

## Non-Persistent Downloads

Each tvOS app is provided a temporary cache directory that its additional resources and assets are downloaded to. This directory will be preserved as long as the app is still running. However, as the Apple TV needs to free up room for other apps or system usage, this cache can be deleted.

As a result, your app cannot rely on previously downloaded content being available the next time it is launched. Your Xamarin.tvOS app should always check for the existence of required resources and download them as required.

> [!IMPORTANT]
> While you have the ability to download other assets and resources as required, Apple warns against consuming all of the space in your app's cache, as it can lead to unpredictable results.

<a name="Managing-Resources"></a>

## Managing Resources

As stated above, because of the limited, non-persistent storage of information available to tvOS apps, these restrictions require careful planning to create a great user experience for your Xamarin.tvOS app.

<a name="iCloud-Data-Storage"></a>

### iCloud Data Storage

Because storage on the Apple TV is limited, not only is there very limited persistent, local storage, your app has no guarantee that any information it previously downloaded will be available the next time it is run.

As a result, your Xamarin.tvOS app must store any user data in an iCloud Data Store. Apple provides two iCloud-based data storage options for your tvOS apps:

- **iCloud Key-Value Storage (KVS)** - For small pieces of information (less than 1MB) that your app might require (like user preferences), you can use iCloud KVS Storage. iCloud KVS data is automatically synced to the cloud and all of the user's devices running the same app. For more information please see the [Key-Value Storage](~/ios/data-cloud/introduction-to-icloud.md) section of our [Introduction to iCloud](~/ios/data-cloud/introduction-to-icloud.md) document or Apple's [Designing for Key-Value Data in iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) documentation.
- **CloudKit** - For the storage of larger pieces of information (greater than 1MB), use Apple's CloudKit Framework. Unlike iCloud KVS Storage, CloudKit data can be shared amongst all the users of your app (as well as being private to a single user). Form more information, please see our [Introduction to CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) documentation or Apple's [CloudKit Quick Start](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

> [!IMPORTANT]
> Apple [provides tools](https://developer.apple.com/support/allowing-users-to-manage-data/) 
> to help developers properly handle the European Union's General Data 
> Protection Regulation (GDPR).

<a name="On-Demand-Resources"></a>

### On-Demand Resources

On-Demand Resources provide app content and assets (separate from the app bundle), that are hosted on the App Store and downloaded as required by your app. Up to an additional 2GB of data can be served using On-Demand Resources. They enable the initial app download to be smaller (tvOS apps are limited to a maximum of 200MB), while still providing rich assets as required.

When a tvOS app requests On-Demand Resources, the system will automatically manage the downloading and storage of this content to the app's cache directory. Your app must wait for this content to be downloaded and made available before it can continue.

These resources may continue to be cached on the Apple TV throughout multiple launches of your app, thus speeding launch cycle. However, your app cannot rely on any previously downloaded content being available the next time it is launched. See the [Non-Persistent Downloads](#Non-Persistent-Downloads) section above for more details.

You use Xcode to create bundles of related content (such as all assets for game level 2) associated with a give Resource Tag. Later your app will request On-Demand Resource by specifying this Resource Tag. Your app should present a UI to the user stating that content is being downloaded. For more information, please see Apple's [On-Demand Resources Guide](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Care should be taken to strike the right balance between the number of times the app has to download On-Demand Resources and the size of the individual downloads. User may become frustrated with your app if gameplay is interrupted constantly to download new content or if a single download takes too much time.

<a name="Summary"></a>

## Summary

This article has covered the size, resource and data storage limitations placed on a Xamarin.tvOS app by the tvOS system. It has presented options to work around these limitations and suggestions to create a great user experience for your app.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)