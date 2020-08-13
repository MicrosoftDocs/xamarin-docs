---
title: "Unified API Overview"
description: "Xamarin's Unified API makes it possible to share code between Mac and iOS and support 32 and 64-bit applications with the same binary."
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
---

# Unified API Overview

Xamarin's Unified API makes it possible to share code between Mac and iOS
and support 32 and 64-bit applications with the same binary. The Unified
API is used by default in new Xamarin.iOS and Xamarin.Mac projects.

> [!IMPORTANT]
> The Xamarin Classic API, which preceded the Unified API, has been
> deprecated.
>
> - The last version of Xamarin.iOS to support the Classic API
>   (monotouch.dll) was Xamarin.iOS 9.10.
> - Xamarin.Mac still supports the Classic API, but it is no longer
>   updated. Since it is deprecated, developers should move their
>   applications to the Unified API.

## Updating Classic API-based Apps

Follow the relevant instructions for your platform:

- [Update Existing Apps](updating-apps.md)
- [Update Existing iOS Apps](updating-ios-apps.md)
- [Update Existing Mac Apps](updating-mac-apps.md)
- [Update Existing Xamarin.Forms Apps](updating-xamarin-forms-apps.md)
- [Migrating a Binding to the Unified API](update-binding.md)

## [Tips for Updating Code to the Unified API](updating-tips.md)

Regardless of what applications you are migrating, check out [these tips](updating-tips.md)
to help you successfully update to the Unified API.

## Library Split

From this point on, our APIs will be surfaced in two ways:

- **Classic API:** Limited to 32-bits (only) and exposed in the `monotouch.dll` and `XamMac.dll` assemblies.
- **Unified API:** Support both 32 and 64 bit development with a single API available in the `Xamarin.iOS.dll` and  `Xamarin.Mac.dll` assemblies.

This means that for Enterprise developers (not targeting the App Store),
you can continue using the existing Classic APIs, as we will keep
maintaining them forever, or you can upgrade to the new APIs.

<a name="namespace-changes"></a>

## Namespace Changes

To reduce the friction to share code between our Mac and
  iOS products, we are changing the namespaces for the APIs in
  the products.

We are dropping the prefix "MonoTouch" from our iOS product
  and "MonoMac" from our Mac product on the data types.

This makes it simpler to share code between the Mac and iOS
  platforms without resorting to conditional compilation and
  will reduce the noise at the top of your source code files.

- **Classic API:** Namespaces use `MonoTouch.` or `MonoMac.` prefix.
- **Unified API:** No namespace prefix

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

## API Changes

The Unified API removes deprecated methods and there are a few instances where there were typos in the API names when they were bound to the original MonoTouch and MonoMac namespaces in the Classic APIs. These instances have been corrected in the new Unified APIs and will need to be updated in your component, iOS and Mac applications. Here is a list of the most common ones you might run into:

|Classic API Method Name|Unified API Method Name|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

For a full list of changes when switching from the Classic to the Unified API, please see our [Classic (monotouch.dll) vs Unified (Xamarin.iOS.dll) API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) documentation.

## Updating to Unified

Several old/broken/deprecated API in **classic** are not
  available in the **Unified** API. It can be easier to
  fix the `CS0616` warnings before starting your (manual
  or automated) upgrade since you'll have the `[Obsolete]`
  attribute message (part of the warning) to guide you to the right API.

Note that we are publishing a [*diff*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
  of the classic vs unified
  API changes that can be used either before or after your project
  updates. Still fixing the obsoletes calls in Classic will often
  be a time saver (less documentation lookups).

Follow these instructions to [update existing iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md),
  or [Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md) to the Unified API.
  Review the remainder of this page, and [these tips](~/cross-platform/macios/unified/updating-tips.md)
  for additional information on migrating your code.

### NuGet

NuGet packages that previously supported Xamarin.iOS via the
  Classic API published their assemblies using the **Monotouch10**
  platform moniker.

The Unified API introduces a new platform identifier for compatible packages -
  **Xamarin.iOS10**. Existing NuGet packages will need to be updated to add
  support for this platform, by building against the Unified API.

> [!IMPORTANT]
> If you have an error in the form _"Error 3 Cannot include both 'monotouch.dll' and 'Xamarin.iOS.dll' in the same Xamarin.iOS project - 'Xamarin.iOS.dll' is referenced explicitly, while 'monotouch.dll' is referenced by 'xxx, Version=0.0.000, Culture=neutral, PublicKeyToken=null'"_ after converting your application to the Unified APIs, it is typically due to having either a component or NuGet Package in the project that has not been updated to the Unified API. You'll need to remove the existing component/NuGet, update to a version that supports the Unified APIs and do a clean build.

### The Road to 64 Bits

For background on supporting 32 and 64 bit applications and
  information about frameworks see the [32 and 64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types"></a>

#### New Data Types

At the core of the difference, both Mac and iOS APIs use an
  architecture-specific data types that are always 32 bit on 32
  bit platforms and 64 bit on 64 bit platforms.

For example, Objective-C maps the `NSInteger`
  data type to `int32_t` on 32 bit systems and
  to `int64_t` on 64 bit systems.

To match this behavior, on our Unified API, we are
  replacing the previous uses of `int` (which in .NET
  is defined as always being `System.Int32`) to a new
  data type: `System.nint`.  You can think of the "n"
  as meaning "native", so the native integer type of the
  platform.

We are introducing `nint`, `nuint`
  and `nfloat` as well providing data types built on
  top of them where necessary.

To learn more about these data type changes,
  see the [Native Types](~/cross-platform/macios/nativetypes.md)
  document.

### How to detect the architecture of iOS apps

There might be situations where your application needs to know if it is running on a 32 bit or a 64 bit iOS system. The following code can be used to check the architecture:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis"></a>

### Arrays and System.Collections.Generic

Because C# indexers expect a type of `int`, you'll have to explicitly cast `nint` values to `int` to access the elements in a collection or array. For example:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

This is expected behavior, because the cast from `int` to `nint` is lossy on 64 bit, an implicit conversion is not done.

### Converting DateTime to NSDate

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

<a name="deprecated-typos"></a>

### Deprecated APIs and Typos

Inside Xamarin.iOS classic API (monotouch.dll) the `[Obsolete]` attribute
  was used in two different ways:

- **Deprecated iOS API:** This is when Apple hints to you to stop using an API because it's being superseded by a newer one. The Classic API is still fine and often required (if you support the older version of iOS).
 Such API (and the  `[Obsolete]` attribute) are included into the new Xamarin.iOS assemblies.
- **Incorrect API** Some API had typos on their names.

For the original assemblies (monotouch.dll and XamMac.dll)
  we kept the old code available for compatibility but they have
  been removed from the Unified API assemblies
  (Xamarin.iOS.dll and Xamarin.Mac)

<a name="NSObject_ctor"></a>

### NSObject subclasses .ctor(IntPtr)

Every `NSObject` subclass has a constructor that accepts an `IntPtr`. This is how we can instantiate a new managed instance from a native ObjC handle.

In classic this was a `public` constructor. However it was easy to misuse this feature in user code, e.g. creating several managed instances for a single ObjC instance *or* creating a managed instance that would lack the expected managed state (for subclasses).

To avoid those kind of problems the `IntPtr` constructors are now `protected` in **unified** API, to be used only for subclassing. This will ensure the correct/safe API is used to create managed instance from handles, i.e.

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

This API will return an existing managed instance (if it already exists) or will create a new one (when required). It is already available in both classic and unified API.

Note that the `.ctor(NSObjectFlag)` is now also `protected` but this one was rarely used outside of subclassing.

<a name="NSAction"></a>

### NSAction Replaced with Action

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

### Custom delegates replaced with Action\<T>

In **unified** some simple (e.g. one parameter) .net delegates were replaced with `Action<T>`. E.g.

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

can now be used as an `Action<NSNotification>`. This promote code reuse and reduce code duplication inside both Xamarin.iOS and your own applications.

### Task\<bool> replaced with Task<Boolean,NSError>>

In **classic** there were some async APIs returning `Task<bool>`. However some of them where are to use when an `NSError` was part of the signature, i.e. the `bool` was already `true` and you had to catch an exception to get the `NSError`.

Since some errors are very common and the return value was not useful this pattern was changed in **unified** to return a `Task<Tuple<Boolean,NSError>>`. This allows you to check both the success and any error that could have happened during the async call.

### NSString vs string

In a few cases some constants had to be changed from `string` to `NSString`, e.g. `UITableViewCell`

**Classic**

```csharp
public virtual string ReuseIdentifier { get; }
```

**Unified**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

In general we prefer the .NET `System.String` type. However, despite Apple guidelines, some native API are comparing constant pointers (not the string itself) and this can only work when we expose the constants as `NSString`.

 <a name="protocols"></a>

### Objective-C Protocols

The original MonoTouch did not have full support for ObjC
  protocols and some, non-optimal, API were added to support the
  most common scenario. This limitation does not exists anymore
  but, for backward compatibility, several APIs are kept around
  inside `monotouch.dll` and `XamMac.dll`.

These limitations have been removed and cleaned up on the
  Unified APIs. Most changes will look like this:

**Classic**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**Unified**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

The `I` prefix means **unified** expose an interface, instead of a specific type, for the ObjC protocol. This will ease cases where you do not want to subclass the specific type that Xamarin.iOS provided.

It also allowed some API to be more precise and easy to use, e.g.:

**Classic**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**Unified**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

Such API are now easier to us, without referring to documentation, and your IDE code completion will provide you with more useful suggestions based on the protocol/interface.

#### NSCoding Protocol

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


## Related Links

- [Updating iOS Apps](updating-ios-apps.md)
- [Updating Mac Apps](updating-mac-apps.md)
- [Updating Xamarin.Forms Apps](updating-xamarin-forms-apps.md)
- [Updating Bindings](update-binding.md)
- [Updating Tips](updating-tips.md)
- [Classic vs Unified API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/native-types-cross-platform.md)
