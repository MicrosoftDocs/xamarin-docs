---
title: "Error when submitting to App Store: “Invalid Bundle - Options not allowed to be embedded in bitcode are detected in the submission”"
description: "Learn about an error when submitting to the App Store: Invalid Bundle - Options not allowed to be embedded in bitcode are detected in the submission."
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
no-loc: [Objective-C]
---

# Error when submitting to App Store: “Invalid Bundle - Options not allowed to be embedded in bitcode are detected in the submission”

watchOS apps and tvOS apps _require_ bitcode when they are submitted
to the App Store. When building and submitting watchOS and tvOS apps using
Xcode 8.3 or earlier, the following error may occur (via email notification)
when attempting to upload to the App Store:

>Invalid Bundle - The app cannot be processed because options not allowed to be embedded in bitcode are detected in the submission. It is likely that you are not building the app with the toolchain provided in Xcode.

The solution to this problem is to build the applications with Xcode 9 and the
latest version of Xamarin.iOS.
