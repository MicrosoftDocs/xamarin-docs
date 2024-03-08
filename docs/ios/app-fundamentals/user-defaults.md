---
title: "Working with User Defaults in Xamarin.iOS"
description: "This article covers working with NSUserDefaults to save default settings in a Xamarin iOS app or extension. It describes NSUserDefaults at a high level and discusses how to read and write values."
ms.service: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
no-loc: [Objective-C]
---

# Working with User Defaults in Xamarin.iOS

_This article covers working with NSUserDefault to save default settings in a Xamarin.iOS App or Extension._

The `NSUserDefaults` class provides a way for iOS Apps and Extensions to programmatically interact with the system-wide Defaults System. By using the Defaults System, the user can configure an app's behavior or styling to meet their preferences (based on the design of the app). For example, to present data in Metric vs Imperial measurements or select a given UI Theme.

When used with App Groups, `NSUserDefaults` also provides a way to communicate between apps (or Extensions) within a given group.

<a name="About-User-Defaults"></a>

## About User Defaults

As stated above, User Defaults (`NSUserDefaults`) can be added to an App (or Extension) and used to provide configurable options that the end user can modify to adjust the look or operation of the app at runtime.

When your app first executes, `NSUserDefaults` reads the keys and values from the app's User Defaults Database and caches them to memory to avoid opening and reading the database each time a value is required. 

> [!IMPORTANT]
> Apple no longer recommends that the developer call the `Synchronize` method to sync the in-memory cache with the database directly. Instead, it will be automatically called at periodic intervals to keep the in-memory cache in sync with a user’s defaults database.

The `NSUserDefaults` class contains several convenience methods for reading and writing preference values for common data types such as: string, integer, float, boolean and URLs. Other types of data can be archived using `NSData`, then read from or written to the User Defaults Database. For more information, please see Apple's [Preferences and Settings Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance"></a>

## Accessing the Shared NSUserDefaults Instance 

The Shared User Defaults Instance provides access to the User Defaults for the current user of the device. If the Shared Defaults object doesn't exist, it is created the first time it is accessed and initialized with the following information:

- An `NSArgumentDomain` consisting of the defaults parsed from the current app.
- The app's Bundle Identifier domain.
- An `NSGlobalDomain` consisting of the defaults shared by all apps.
- A separate domain for each of the user's preferred languages.
- An `NSRegistrationDomain` with a set of temporary defaults that can be modified by the app to ensure searches are always successful.

To access the Shared User Defaults Instance, use the following code:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance"></a>

## Accessing an App Group NSUserDefaults Instance

As stated above, by using App Groups, `NSUserDefaults` can be used to communicate between Apps (or Extensions) within a given group. First, you will need to ensure that the App Group and the required App IDs have been properly configured in the **Certificates, Identifiers & Profiles** section on iOS Dev Center and have been installed in the development environment.

Next, your App and/or Extension projects need to have one of the valid App IDs created above, and the `Entitlements.plist` file has to be included in the App Bundle with the App Groups enabled and specified.

With this all in place, the shared App Group User Defaults can be accessed using the following code:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Where `group.com.xamarin.todaysharing` is the App Group created in **Certificates, Identifiers & Profiles** that you want to access. For more information, please see the [App Group Capabilities](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

<a name="Reading-Default-Values"></a>

## Reading Default Values

After you have accessed the desired User Default Database, you can read values from the defaults using Key/Value Pairs and several convenience methods based on the type of data being read:

- `ArrayForKey` - Returns an array of `NSObjects` for the given key value.
- `BoolForKey` - Returns a boolean value for the given key.
- `DataForKey` - Returns an `NSData` object for the given key.
- `DictionaryForKey` - Returns an `NSDictionary` for the given key.
- `DoubleForKey` - Returns a double value for the given key.
- `FloatForKey` - Returns a float value for the given key.
- `IntForKey` - Returns an integer value for the given key.
- `StringArrayForKey` - Returns an array of `String` objects from the given key value.
- `StringForKey` - Returns a string value for the given key.
- `URLForKey` - Returns an `NSUrl` value for the given key.

For example, the following code would read a boolean value from the User Defaults:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values"></a>

## Writing Default Values

Just like reading values above, after you have accessed the desired User Default Database, you can write values to the defaults using Key/Value Pairs and several convenience methods based on the type of data being written:

- `SetBool` - Writes the given boolean value to the given key.
- `SetDouble` - Writes the given double value to the given key.
- `SetFloat` - Writes the given float value to the given key.
- `SetString` - Writes the given string value to the given key.
- `SetURL` - Writes the given URL (`NSUrl`) value to the given key.

For example, the following code would write a boolean value to the User Defaults:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> When your App first executes, `NSUserDefaults` reads the keys and values from the app's User Defaults Database and caches them to memory to avoid opening and reading the database each time a value is required.

<a name="Summary"></a>

## Summary

This article has covered the `NSUserDefaults` class and how it can be used to provide a set of options that the end user can use to configure your Xamarin.iOS App. Additionally, it covered using App Groups to communicate between an extension and its Parent App or between apps in a group.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [Preferences and Settings Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)