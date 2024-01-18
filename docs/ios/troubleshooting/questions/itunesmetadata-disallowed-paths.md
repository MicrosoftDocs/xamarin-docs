---
title: "Why does my app submission fail with: Disallowed paths ( iTunesMetadata.plist ) found at ... ?"
description: Learn about the ITMS-90047 Disallowed paths ("iTunesMetadata.plist") error received when submitting a Xamarin application for review in the AppStore.
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
no-loc: [Objective-C]
---

# Why does my app submission fail with: Disallowed paths ( iTunesMetadata.plist ) found at ... ?

> ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"

This error is the result of a change in Apple's App Store verification process to prevent users from hitting issues like [Bug 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180). This specific error is _not_ related to the particular version of Xamarin you have installed, so downgrading will _not_ help.

See the forum discussion [here](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) for workaround information and the latest updates for this issue.
