---
title: "iOS 9 Compatibility"
description: "Even if you don't plan to add iOS 9 features to your app straight away, you should rebuild your apps with the latest version of Xamarin."
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
---

# iOS 9 Compatibility

_Even if you don't plan to add iOS 9 features to your app straight away, you should rebuild your apps with the latest version of Xamarin._

> [!IMPORTANT]
> The information on this page is for customers with apps already in the App
> Store targeting iOS 8 or earlier, who have not already submitted updates
> for iOS 9 compatibility. If you are already using the latest versions -
> Xcode 7 and Xamarin.iOS 9 - for your app development please visit the
> [introduction to iOS 9](~/ios/platform/introduction-to-ios9/index.md).

When the first iOS 9 betas appeared, we identified two issues with older versions of
Xamarin that manifested as older apps being unable to start on iOS 9:

- Apps build for iOS 8 or earlier not being able to start on 32-bit devices (including apps built with the [Unified API](~/cross-platform/macios/unified/index.md)).
- P/Invoke failing with the full path is not specified.

Updating your Xamarin installation to the latest Stable Channel release,
and then rebuilding and redeploying your apps, fixes these two issues.

_Even if you aren't planning to update your app with iOS 9 features right away, we recommend
you re-build with the latest version of Xamarin and re-submit to the App Store_.

This will ensure that your app will run on iOS 9 after your customers upgrade.
You can continue to support iOS 8 - rebuilding with the latest release does
not affect the application target version.

If you have further issues while testing your existing apps on iOS 9,
read the [Improving Compatibility](#compat) section below.

### Updating with Visual Studio

It is recommended that you explicitly check that Visual Studio is updated to the latest Stable version.

## What about Components, Nugets, and other libraries?

You do **not** need to wait for new versions of any Components or
Nugets you are using to address the two issues mentioned above.
These issues are fixed simply by re-building your app with the
latest Stable release of Xamarin.iOS.

Similarly, Component vendors and NuGet authors are **not** required to submit
new builds just to fix the two issues mentioned above. However, if any a
Component or NuGet uses `UICollectionView` or load views from **Xib** files, an update
*may* be required to address the iOS 9 compatibility issues mentioned below.

<a name="compat"></a>

## Improving Compatibility in Your Code

There's a few cases of code patterns that *used* to work in older versions of iOS breaking in iOS 9. Here are some possible issues (and their solutions) that may arise when testing on iOS 9:

### UICollectionViewCell.ContentView is null in constructors

**Reason:** In iOS 9 the `initWithFrame:` constructor is now required, due to behavior changes in iOS 9 as the [UICollectionView documentation states](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). If you registered a class for the specified identifier and a new cell must be created, the cell is now initialized by calling its `initWithFrame:` method.

**Fix:** Add the `initWithFrame:` constructor like this:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Related samples: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

### UIView fails to init with coder when loading a view from a Xib/Nib

**Reason:** The `initWithCoder:` constructor is the one called when loading a view from an Interface Builder Xib file. If this constructor is not exported unmanaged code can’t call our managed version of it. Previously (eg. in iOS 8) the `IntPtr` constructor was invoked to initialize view.

**Fix:** Create and export the `initWithCoder:` constructor like this:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Related sample: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

### Dyld Message: no cache image with name...

You might experience a crash with the following information in the log:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Reason:** This is a bug in Apple's native linker, which happens when they make a private framework public
(JavaScriptCore was made public in iOS 7, before that it was a private framework), and the deployment target
of the app is for an iOS version when the framework was private. In this case Apple's linker will link with the
private version of the framework instead of the public version.

**Fix:** This will be addressed for iOS 9, but there is an easy workaround you can apply yourself in the meantime:
just target a later iOS version in your project (you can try iOS 7 in this case). Other frameworks might exhibit
similar problems, for example the WebKit framework was made public in iOS 8 (and so targeting iOS 7 will result in
this error; you should target iOS 8 to use WebKit in your app).

## Related Links

- [iOS 9 compatibility release info](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Introduction to iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Updating your Xamarin.iOS apps to iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
