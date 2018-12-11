---
title: "Working with the watchOS Parent Application in Xamarin"
description: "This document describes how to work with a watchOS parent application in Xamarin. It discusses WatchKit app extensions, iOS apps, shared storage, and more."
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
---

# Working with the watchOS Parent Application in Xamarin

> [!IMPORTANT]
> Accessing the parent application using the examples below
only works on watchOS 1 watch apps.


There are different ways to communicate between the watch
	app and the iOS app that it is bundled with:

- Watch extensions can [call a method](#code) against the parent app
	that runs in the background on the iPhone.

- Watch extensions can [share a storage location](#storage)
	with the parent iPhone app.

- Using Handoff to pass data from a Glance or
	Notification to the Watch app, sending the user
	to a specific interface controller in the app.

The Parent App is also sometimes referred to as the Container App.


<a name="code" />

## Run Code

Communicating between a watch extension and the parent
	iPhone app is demonstrated in the [GpsWatch sample](https://developer.xamarin.com/samples/GpsWatch).
	Your watch extension can request the parent iOS app
	to do some processing on its behalf using the `OpenParentApplication`
	method.

This is especially useful for long running tasks (including
	network requests) - only the parent iOS app can take advantage of
	background processing to complete these tasks and save the
	retrieved data in a location accessible to the watch extension.



### Watch Kit App Extension

Call the `WKInterfaceController.OpenParentApplication` in your watch
	app extension. It returns a `bool` that indicates whether the
	method request was sent successfully or not. Check the `error`
	parameter in the response `Action` to determine if any occurred during
	the method running in the iPhone app.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
	if(error != null) {
		Console.WriteLine (error);
		return;
	}
	Console.WriteLine ("parent app responded");
	// do something with replyInfo[] dictionary
});
```


### iOS App

All calls from a watch app extension are routed through
	the iPhone app's `HandleWatchKitExtensionRequest` method.
	If you are making different requests in the watch app
	then this method will need to query the `userInfo` dictionary
	to determine how to process the request.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
	// ... other AppDelegate methods
	public override void HandleWatchKitExtensionRequest
		(UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
	{
		var status = 2;
		// do something in the background, and respond
		reply (new NSDictionary (
			"count", NSNumber.FromInt32 ((int)status),
			"value2", new NSString("some-info")
			));
	}
}
```


<a name="storage" />

## Shared Storage

If you configure an [app group](~/ios/watchos/app-fundamentals/app-groups.md)
	then iOS 8 extensions (including watch extensions) can share data
	with the parent app.

<a name="nsuserdefaults" />

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

<a name="files" />

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

## WormHoleSharp

A popular open-source mechanism for watchOS 1 (based on [MMWormHole](https://github.com/mutualmobile/MMWormhole))
	to pass data or commands between the parent
	app and the watch app.

You can configure WormHole using an app group like this
	in your iOS app and watch extension:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Download the C# version [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## Related Links

- [GpsWatch (sample)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WormHoleSharp (sample)](https://github.com/Clancey/WormHoleSharp)
- [Apple's WKInterfaceController reference](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple's Sharing Data with Your Containing App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
