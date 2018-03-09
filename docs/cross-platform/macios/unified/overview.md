---
title: "Unified API Overview"
description: "The new style API makes it easier than ever to share code between Mac and iOS as well as allowing you to support 32 and 64 bit applications with the same binary."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
---

# Unified API Overview

_The new style API makes it easier than ever to share code between Mac and iOS as well as allowing you to support 32 and 64 bit applications with the same binary._

To improve code sharing between Mac and iOS and to enable
	developers to have a single code base that works on 32 and 64
	bits, in early 2015 we introduced a new API in both Xamarin.Mac and
	Xamarin.iOS products called the Unified API.

> [!IMPORTANT]
> **Classic Profile Deprecation:** As new platforms are added in Xamarin.iOS we are starting to gradually deprecate features from the classic profile (monotouch.dll). For example, the non-NRC (new-ref-count) option was removed. NRC has always been enabled for all unified applications (i.e. non-NRC was never an option) and has no known issues. Future releases will remove the option of using Boehm as the garbage collector. This was also an option never available to unified applications. The complete removal of classic support is scheduled for next fall with the release of Xamarin.iOS 10.0.

## iOS

The `Xamarin.iOS.dll` assembly shipped with Xamarin.iOS 8.6 is our
	**first stable and supported version** of the Unified API for iOS.
	Previous preview versions of the unified API are close but not fully
	compatible.

## Mac

The `Xamarin.Mac.dll` assembly in the stable channel of Xamarin.Mac is our
**first stable and supported version** of the Unified API for Mac.
Previous preview versions of the unified API are close but not fully compatible.

## Runtime Defaults

The Unified API by default uses the **SGen** garbage
	collector and the [New Reference Counting](~/ios/internals/newrefcount.md)
	system for tracking object ownership. This same feature has been
	ported to Xamarin.Mac.

This solves a number of problems that developers faced with
	the old system and also ease [memory management](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Note that it is possible to enable New Refcount even for
	the Classic API, but the default is conservative and does not
	require users to make any changes. With the Unified API, we
	took the opportunity of changing the default and give
	developers all the improvements at the same time that they
	refactor and re-test their code.

<a name="namespace-changes" />

## Library Split

From this point on, our APIs will be surfaced in two ways:

-  **Classic API:** Limited to 32-bits (only) and exposed in the `monotouch.dll` and `XamMac.dll` assemblies.
-  **Unified API:** Support both 32 and 64 bit development with a single API available in the `Xamarin.iOS.dll` and  `Xamarin.Mac.dll` assemblies.

This means that for Enterprise developers (not targetting the App Store),
you can continue using the existing Classic APIs, as we will keep
maintaining them forever, or you can upgrade to the new APIs.

### Namespace Changes

To reduce the friction to share code between our Mac and
	iOS products, we are changing the namespaces for the APIs in
	the products.

We are dropping the prefix "MonoTouch" from our iOS product
	and "MonoMac" from our Mac product on the data types.

This makes it simpler to share code between the Mac and iOS
	platforms without resorting to conditional compilation and
	will reduce the noise at the top of your source code files.

-  **Classic API:** Namespaces use `MonoTouch.` or `MonoMac.` prefix.
-  **Unified API:** No namespace prefix

### API Changes

The Unified API removes deprecated methods and there are a few instances where there were typos in the API names when they were bound to the original MonoTouch and MonoMac namespaces in the Classic APIs. These instances have been corrected in the new Unified APIs and will need to be updated in your component, iOS and Mac applications. Here is a list of the most common ones you might run into:

<table width="100%" border="1">
<tr>
	<th>Classic API Method Name</th>
	<th>Unified API Method Name</th>
</tr>
<tr>
	<td>UINavigationController.PushViewControllerAnimated()</td>
	<td>UINavigationController.PushViewController()</td>
</tr>
<tr>
	<td>UINavigationController.PopViewControllerAnimated()</td>
	<td>UINavigationController.PopViewController()</td>
</tr>
<tr>
	<td>CGContext.SetRGBFillColor()</td>
	<td>CGContext.SetFillColor()</td>
</tr>
<tr>
	<td>NetworkReachability.SetCallback()</td>
	<td>NetworkReachability.SetNotification()</td>
</tr>
<tr>
	<td>CGContext.SetShadowWithColor</td>
	<td>CGContext.SetShadow</td>
</tr>
<tr>
	<td>UIView.StringSize</td>
	<td>UIKit.UIStringDrawing.StringSize</td>
</tr>
</table>

For a full list of changes when switching from the Classic to the Unified API, please see our [Classic (monotouch.dll) vs Unified (Xamarin.iOS.dll) API differences](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) documentation.

### Updating to Unified

Several old/broken/deprecated API in **classic** are not
	available in the **Unified** API. It can be easier to
	fix the `CS0616` warnings before starting your (manual
	or automated) upgrade since you'll have the `[Obsolete]`
	attribute message (part of the warning) to guide you to the right API.

Note that we are publishing a [*diff*](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
	of the classic vs unified
	API changes that can be used either before or after your project
	updates. Still fixing the obsoletes calls in Classic will often
	be a time saver (less documentation lookups).

Follow these instructions to [update existing iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md),
	or [Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md) to the Unified API.
	Review the remainder of this page, and [these tips](~/cross-platform/macios/unified/updating-tips.md)
	for additional information on migrating your code.

## Components and NuGet

Most existing components and NuGet packages
	will need to be updated to support the Unified API.
	Components built against the Classic API cannot be
	referenced from a Unified API project.

Existing components that did not have any references to `monotouch.dll`
(or `XamMac.dll`) should not require updates.

### Components

iOS components in the [Xamarin Component Store](https://components.xamarin.com/)
	must be updated to work with Unified API projects. Xamarin is working to
	update the components we publish and encourage other authors to do the same.

### NuGet

NuGet packages that previously supported Xamarin.iOS via the
	Classic API published their assemblies using the **Monotouch10**
	platform moniker.

The Unified API introduces a new platform identifier for compatible packages -
	**Xamarin.iOS10**. Existing NuGet packages will need to be updated to add
	support for this platform, by building against the Unified API.

> [!IMPORTANT]
> **NOTE:** If you have an error in the form _"Error 3 Cannot include both 'monotouch.dll' and 'Xamarin.iOS.dll' in the same Xamarin.iOS project - 'Xamarin.iOS.dll' is referenced explicitly, while 'monotouch.dll' is referenced by 'xxx, Version=0.0.000, Culture=neutral, PublicKeyToken=null'"_ after converting your application to the Unified APIs, it is typically due to having either a component or NuGet Package in the project that has not been updated to the Unified API. You'll need to remove the existing component/NuGet, update to a version that supports the Unified APIs and do a clean build.

<a name="deprecated-apis" />

## Arrays and System.Collections.Generic

Because C# indexers expect a type of `int`, you'll have to explicitly cast
`nint` values to `int` to access the elements in a collection or array. For example:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
	return Names[(int)index];
}

```

This is expected behavior, because the cast from `int` to `nint` is lossy on
64 bit, an implicit conversion is not done.

## Converting DateTime to NSDate

When using the Unified APIs, the implicit conversion of `DateTime` to `NSDate` values is no longer performed. These values will need to be explicitly converted from one type to another. The following extension methods can be used to automate this process:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
	// NSDate has a wider range than DateTime, so clip
	// the converted date to DateTime.Min|MaxValue.
	double secs = date.SecondsSinceReferenceDate;
	if (secs < -63113904000)
		return DateTime.MinValue;
	if (secs > 252423993599)
		return DateTime.MaxValue;
	return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
	if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

## Deprecated APIs and Typos

Inside Xamarin.iOS classic API (monotouch.dll) the `[Obsolete]` attribute
	was used in two different ways:

-  **Deprecated iOS API:** This is when Apple hints to you to stop using an API because it's being superseded by a newer one. The Classic API is still fine and often required (if you support the older version of iOS).
 Such API (and the  `[Obsolete]` attribute) are included into the new Xamarin.iOS assemblies.
-  **Incorrect API** Some API had typos on their names.

For the original assemblies (monotouch.dll and XamMac.dll)
	we kept the old code available for compatibility but they have
	been removed from the Unified API assemblies
	(Xamarin.iOS.dll and Xamarin.Mac)

<a name="NSObject_ctor" />

## NSObject subclasses .ctor(IntPtr)

Every `NSObject` subclass has a constructor that accepts an `IntPtr`. This is how we can instantiate a new managed instance from a native ObjC handle.

In classic this was a `public` constructor. However it was easy to misuse this feature in user code, e.g. creating several managed instances for a single ObjC instance *or* creating a managed instance that would lack the expected managed state (for subclasses).

To avoid those kind of problems the `IntPtr` constructors are now `protected` in **unified** API, to be used only for subclassing. This will ensure the correct/safe API is used to create managed instance from handles, i.e.

    var label = Runtime.GetNSObject<UILabel> (handle);

This API will return an existing managed instance (if it already exists) or will create a new one (when required). It is already available in both classic and unified API.

Note that the `.ctor(NSObjectFlag)` is now also `protected` but this one was rarely used outside of subclassing.

<a name="NSAction" />

## NSAction Replaced with Action

With the Unified APIs, `NSAction` has been removed in favor of the standard .NET `Action`. This is a big improvement because `Action` is a common .NET type, whereas `NSAction` was specific to Xamarin.iOS. They both do exactly the same thing, but they were distinct and incompatible types and resulted in more code having to be written to achieve the same result.

For example, if your existing Xamarin application included the following code:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
	ShowDropDownAnimated (tblDataView);
}));
```
It can now be replaced with a simple lambda:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Previously that would be a compiler error because an `Action` can't be assigned to `NSAction`, but since `UITapGestureRecognizer` now takes an `Action` instead of an `NSAction` it is valid in the Unified APIs.

## Custom delegates replaced with Action<T>

In **unified** some simple (e.g. one parameter) .net delegates were replaced with `Action<T>`. E.g.

	public delegate void NSNotificationHandler (NSNotification notification);

can now be used as an `Action<NSNotification>`. This promote code reuse and reduce code duplication inside both Xamarin.iOS and your own applications.

## Task<bool> replaced with Task<Boolean,NSError>>

In **classic** there were some async APIs returning `Task<bool>`. However some of them where are to use when an `NSError` was part of the signature, i.e. the `bool` was already `true` and you had to catch an exception to get the `NSError`.

Since some errors are very common and the return value was not useful this pattern was changed in **unified** to return a `Task<Tuple<Boolean,NSError>>`. This allows you to check both the success and any error that could have happened during the async call.

## NSString vs string

In a few cases some constants had to be changed from `string` to `NSString`, e.g. `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

In general we prefer the .NET `System.String` type. However, despite Apple guidelines, some native API are comparing constant pointers (not the string itself) and this can only work when we expose the constants as `NSString`.

 <a name="protocols" />

## Objective-C Protocols

The original MonoTouch did not have full support for ObjC
	protocols and some, non-optimal, API were added to support the
	most common scenario. This limitation does not exists anymore
	but, for backward compatibility, several APIs are kept around
	inside `monotouch.dll` and `XamMac.dll`.

These limitations have been removed and cleaned up on the
	Unified APIs. Most changes will look like this:

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

The `I` prefix means **unified** expose an interface, instead of a specific type, for the ObjC protocol. This will ease cases where you do not want to subclass the specific type that Xamarin.iOS provided.

It also allowed some API to be more precise and easy to use, e.g.:

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Such API are now easier to us, without refering to documentation, and your IDE code completion will provide you with more useful suggestions based on the protocol/interface.

### NSCoding Protocol

Our original binding included an .ctor(NSCoder) for every
	type - even if it did not support the `NSCoding`
	protocol.  A single `Encode(NSCoder)` method was
	present in the `NSObject` to encode the object.
	But this method would only work if the instance conformed to
	NSCoding protocol.

On the Unified API we have fixed this.  The new
	assemblies will only have the `.ctor(NSCoder)` if
	the type conforms to `NSCoding`. Also such types
	now have an `Encode(NSCoder)` method which conforms
	to the `INSCoding` interface.

Low Impact: In most cases this change won’t affect applications as
	the old, removed, constructors could not be used.

## Further Tips

Additional changes to be aware of are listed in the
	[tips for updating apps to the Unified API](~/cross-platform/macios/unified/updating-tips.md).

## Sample Code

As of July 31st, we have published ports of the iOS samples
	to this new API on the `magic-types` branch
	at [monotouch-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

For Mac, we are checking samples in both
	the [mac-samples](https://github.com/xamarin/mac-samples)
	repo (showing new APIs in Mavericks/Yosemite) as well as 32/64
	bit samples in the magic-types
	branch [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## Related Links

- [Updating iOS Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Updating Mac Apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Updating Xamarin.Forms Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Updating Bindings](~/cross-platform/macios/unified/update-binding.md)
- [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Updating Tips](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs Unified API differences](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
