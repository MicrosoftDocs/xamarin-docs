---
title: "Working with the watchOS Parent Application in Xamarin"
description: "This document describes how to work with a watchOS parent application in Xamarin. It discusses watchOS app extensions, iOS apps, shared storage, and more."
ms.service: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
no-loc: [Objective-C]
---

# Working with the watchOS Parent Application in Xamarin

There are different ways to communicate between the watch app and the iOS app that it is bundled with:

- Watch apps can [run code](#run-code) on the parent app on the iPhone.

- Watch extensions can [share a storage location](#shared-storage) with the parent iPhone app.

- Use handoff to pass data from a notification to the watch app, sending the user to a specific interface controller in the app.

The Parent App is also sometimes referred to as the Container App.

## Run Code

These two samples demonstrate how to use `WCSession` to run code and send messages between a watch app and the paired iPhone:

- [Watch Connectivity](/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [SimpleWatchConnectivity](/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## Shared Storage

If you configure an [app group](~/ios/watchos/app-fundamentals/app-groups.md)
then iOS 8 extensions (including watch extensions) can share data
with the parent app.

### NSUserDefaults

The following code can be written in both the watch app
extension and the parent iPhone app so that they can
reference a common set of `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files"></a>

### Files

The iOS app and watch extension can also share files
using a common file path.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Note: if the path is `null` then check the
[app group configuration](~/ios/watchos/app-fundamentals/app-groups.md)
to ensure the provisioning profiles have been configured correctly
and have been downloaded/installed on the development computer.

For more information, please see the [App Group Capabilities](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

## Related Links

- [Apple's WKInterfaceController reference](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple's Sharing Data with Your Containing App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)