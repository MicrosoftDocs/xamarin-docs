---
title: "Tips for Updating Code to the Unified API"
description: "This document discusses common errors and various tips useful when updating an application to use Xamarin's Unified API."
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: asb3993
ms.author: amburns
---

# Tips for Updating Code to the Unified API

When updating older Xamarin solutions to the Unified API, the following errors might be encountered.

## NSInvalidArgumentException Could Not Find storyboard Error

There is a [bug](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) in the current version of Visual Studio for Mac that can occur after using the automated migration tool to convert your project to the Unified APIs. After the update, if you get an error message in the form:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

You can do the following to solve this issue, locate the following build target file:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

In this file you need to find the following target declaration:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

And add the `DependsOnTargets="_CollectBundleResources"` attribute to it. Like this:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Save the file, reboot Visual Studio for Mac, and do a clean & rebuild of your project. A fix for this issue should be released by Xamarin shortly.

## Useful Tips

After using the migration tool, you may still get some
	compiler errors requiring manual intervention.
	Some things that might need to be manually fixed include:

* Comparing `enum`s might require an `(int)` cast.

* `NSDictionary.IntValue` now returns an `nint`, there is
	an `Int32Value` which can be used instead.

* `nfloat` and `nint` types cannot be marked `const`;
	`static readonly nint` is a reasonable alternative.

* Things that used to be directly in the `MonoTouch.`
	namespace are now generally in the `ObjCRuntime.`
	namespace (e.g.: `MonoTouch.Constants.Version` is now `ObjCRuntime.Constants.Version`).

* Code that serializes objects may break when attempting
	to serialize `nint` and `nfloat` types. Be sure to
	check your serialization code works as expected after migration.

* Sometimes the automated tool misses code inside
	`#if #else` conditional compiler directives. In this case
	you'll need to make the fixes manually (see the common errors
	below).

* Manually exported methods using `[Export]` may not be automatically
	fixed by the migration tool, for example in
	this code snippert you must manually update the return type
	to `nfloat`:

	```csharp
	[Export("tableView:heightForRowAtIndexPath:")]
	public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
	```

 * The Unified API does not provide an implicit conversion between NSDate
 	and .NET DateTime because it's not a lossless conversion. To prevent
 	errors related to `DateTimeKind.Unspecified` convert the .NET `DateTime`
 	to local or UTC before casting to `NSDate`.

 * Objective-C category methods are now generated as extension
 	methods in the Unified API. For example, code that previously
 	used `UIView.DrawString` would now reference
 	`NSString.DrawString` in the Unified API.

 * Code using AVFoundation classes with `VideoSettings` should change
 	to use the `WeakVideoSettings` property. This requires a `Dictionary`,
 	which is available as a property on the settings classes, for example:

	```csharp
	vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
	```

 * The NSObject `.ctor(IntPtr)` constructor has been changed from public
 	to protected ([to prevent improper use](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

 * `NSAction` has been [replaced](~/cross-platform/macios/unified/overview.md#NSAction)
 	with the starndard .NET `Action`. Some simple (single parameter) delegates
 	have also been replaced with `Action<T>`.

Finally, refer to the [Classic v Unified API differences](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
 	to look up changes to APIs in your code. Searching [this page](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
 	will help find Classic APIs and what they've been updated to.

**Note:** the `MonoTouch.Dialog` namespace remains the same
	after migration. If your code uses **MonoTouch.Dialog**
	you should continue to use that namespace - do *not*
	change `MonoTouch.Dialog` to `Dialog`!

## Common Compiler Errors

Other examples of common errors are listed below, along with the solution:

**Error CS0012: The type 'MonoTouch.UIKit.UIView' is defined in an assembly that is not referenced.**

Fix: This usually means the project references a component or NuGet package that has not been built with the Unified API. You should delete and re-add all Components and NuGet packages. If this does not fix the error, the external library may not yet support the Unified API.

**Error MT0034: Cannot include both 'monotouch.dll' and 'Xamarin.iOS.dll' in the same Xamarin.iOS project - 'Xamarin.iOS.dll' is referenced explicitly, while 'monotouch.dll' is referenced by 'Xamarin.Mobile, Version=0.6.3.0, Culture=neutral, PublicKeyToken=null'.**

Fix: Delete the component that is causing this error and re-add to the project.

**Error CS0234: The type or namespace name 'Foundation' does not exist in the namespace 'MonoTouch'. Are you missing an assembly reference?**

Fix: The automated migration tool in Visual Studio for Mac *should* update all `MonoTouch.Foundation` references to `Foundation`, however in some instances these will need to be updated manually. Similar errors may appear for the other namespaces previously contained in `MonoTouch`, such as `UIKit`.

**Error CS0266: Cannot implicitly convert type 'double' to 'System.float'**

Fix: change type and cast to `nfloat`. This error may also occur for the other types with 64-bit equivalents (such as `nint`, )

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Error CS0266: Cannot implicitly convert type 'CoreGraphics.CGRect' to 'System.Drawing.RectangleF'. An explicit conversion exists (are you missing a cast?)**

Fix: Change instances to `RectangleF` to `CGRect`, `SizeF` to `CGSize`, and `PointF` to `CGPoint`. The namespace `using System.Drawing;` should be replaced with `using CoreGraphics;` (if it isn't already present).

**error CS1502: The best overloaded method match for 'CoreGraphics.CGContext.SetLineDash(System.nfloat, System.nfloat[])' has some invalid arguments**

Fix: Change array type to `nfloat[]` and explicitly cast `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Error CS0115: `WordsTableSource.RowsInSection(UIKit.UITableView, int)' is marked as an override but no suitable method found to override**

Fix: Change the return value and parameter types to `nint`. This commonly occurs in method overrides such as those on `UITableViewSource`, including `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`, etc.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Error CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

Fix: When the return type is changed to `nint`, cast the return value to `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
	return (nint)navItems.Count;
}
```

**Error CS1061: Type 'CoreGraphics.CGPath' does not contain a definition for 'AddElipseInRect'**

Fix: Correct spelling to `AddEllipseInRect`. Other name changes include:

* Change 'Color.Black' to `NSColor.Black`.
* Change MapKit 'AddAnnotation' to `AddAnnotations`.
* Change AVFoundation 'DataUsingEncoding' to `Encode`.
* Change AVFoundation 'AVMetadataObject.TypeQRCode' to `AVMetadataObjectType.QRCode`.
* Change AVFoundation 'VideoSettings' to `WeakVideoSettings`.
* Change PopViewControllerAnimated to `PopViewController`.
* Change CoreGraphics 'CGBitmapContext.SetRGBFillColor' to `SetFillColor`.

**Error CS0546: cannot override because `MapKit.MKAnnotation.Coordinate' does not have an overridable set accessor (CS0546)**

When creating a custom annotation by subclassing MKAnnotation the Coordinate field has no setter, only a getter.

[Fix](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Add a field to keep track of the coordinate
* return this field in the getter of the Coordinate property
* Override the SetCoordinate method and set your field
* Call SetCoordinate in your ctor with the passed in coordinate parameter

It should look similar to the following:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## Related Links

- [Updating Apps](~/cross-platform/macios/unified/updating-apps.md)
- [Updating iOS Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Updating Mac Apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Updating Xamarin.Forms Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Updating Bindings](~/cross-platform/macios/unified/update-binding.md)
- [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs Unified API differences](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
