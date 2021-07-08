---
title: "watchOS Project References in Xamarin"
description: "This document describes the relationship between an iOS app, a watch app, and a watch app extension. It discusses project references and bundle identifiers."
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
---

# watchOS Project References in Xamarin

_Explanation of the relationship between the iOS app, watch app, and watch extension._

The three projects in a watchOS solution are *automatically configured* to reference each other
  in a specific way for watchOS 3 apps to be
  built and bundled correctly. These project references and bundle identifier settings
  are described below for reference.

## Project References

View the references by double-clicking on the References
  nodes for each project:

- **iPhone app** references **Watch App**

  ![Screenshot shows the Projects tab.](project-references-images/catalog-reference1.png)

- **Watch App** references **Watch App Extension**

  ![Screenshot shows the Projects tab with MyWatchApp dot OnWatchExtension selected.](project-references-images/catalog-reference2.png)

- The **Watch App Extension** does not reference either of the other projects

  ![Watch App Extension does not reference the other projects](project-references-images/catalog-reference3.png)

## Bundle Identifiers

You also need to make sure your **Bundle Identifiers** are correct.
  All three projects should have the *same* identifier prefix,
  with the two watch projects having predefined extensions of
  `watchkitextension` and `watchkitapp`, as follows
  (for the **WatchKitCatalog** example):

- Xamarin.iOS Unified project - `com.xamarin.WatchKitCatalog`

- WatchKit Extension project  - `com.xamarin.WatchKitCatalog.watchkitextension`

- Watch App project - `com.xamarin.WatchKitCatalog.watchkitapp`

Also make sure that these **Info.plist** settings are correct:

- The Watch App project's
  `WKCompanionAppBundleIdentifier` matches the parent/container
  app's Bundle ID (ie. the one that runs on the iPhone);

- The Watch Kit Extension project's
  **WKApp Bundle ID** matches the Watch App project’s
  Bundle ID.

You can edit the identifiers by double-clicking
  on the **Info.plist** file in each project.

This screenshot
  is the **Watch Extension's** Info.plist file, showing the
  **Watch App's** identifier as well:

# [Visual Studio for Mac](#tab/macos)

![This screenshot is the Watch Extension's Info.plist file](project-references-images/infoplist-extension.png)

# [Visual Studio](#tab/windows)

![This screenshot is the Watch Extension's Info.plist file](project-references-images/infoplist-extension-vs.png)

-----

This screenshot is the **Watch App's** Info.plist file.
  The current **Watch OS** version is 8.2, so the
  **Deployment Target** for the Watch App should be
  **8.2**. Note that if you have Xcode 6.3 installed,
  this value might be set to 8.3 - you should change
  it 8.2.

![The watch Info.plist file](project-references-images/infoplist-watchapp.png)

The deployment target for the Watch App can be
  different from the Watch Extension and iOS App.
