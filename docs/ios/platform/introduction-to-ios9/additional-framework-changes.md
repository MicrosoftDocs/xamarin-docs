---
title: "Additional iOS 9 Frameworks Changes"
description: "This document describes additional framework changes introduced in iOS 9. It discusses AVFoundation, AVKit, and CloudKit."
ms.service: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
no-loc: [Objective-C]
---

# Additional iOS 9 Frameworks Changes

_This article covers additional, minor changes or enhancements to existing frameworks for iOS 9._

[![iOS 9 Logo](additional-framework-changes-images/ios9-sml.png)](additional-framework-changes-images/ios9.png#lightbox)

In addition to the major changes to iOS, Apple has made modifications and improvements to several existing frameworks in iOS 9.

## AVFoundation Framework Additions

In the AVFoundation framework, the [AVSpeechSynthesisVoice](xref:AVFoundation.AVSpeechSynthesisVoice) class now allows you to specify a voice by identifier in addition to language.

For example, the following code gets a list of all available voices:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

You can then use one of the voices from the list by setting it as the `Voice` property of an instance of the [AVSpeachUtterance](xref:AVFoundation.AVSpeechUtterance) class.

The [AVQueuePlayer](xref:AVFoundation.AVQueuePlayer) class now supports a mixture of internet streaming and file-based media in the queue. Previous versions could only queue media of the same type.

For more information, please see Apple's [AVSpeechSynthesisVoice Reference](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## AVKit Framework Additions

To work with the new Picture-in-Picture (PIP) feature, the AVKit framework includes the new `AVPictureInPictureController` and [AVPlayerViewController](xref:AVKit.AVPlayerViewController) classes:

- **AVPictureInPictureController** - This class allows an iOS 9 app to respond to the user launching the playback of a video in a floating, resizable PIP window on an iPad.
- **AVPlayerViewController** - Manages a `AVPlayer` controller used to present a video in a floating, resizable PIP window on an iPad.

For more information, please see our [Multitasking for iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) documentation and Apple's [AVPictureInPictureController Reference](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) and [AVPlayerViewController Reference](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## Introducing CloudKit Web Services

The CloudKit framework streamlines the development of applications that access iCloud. This includes the retrieval of application data and asset rights as well as being able to securely store application information. This kit gives users a layer of anonymity by allowing access to applications with their iCloud IDs without sharing personal information.

The new _CloudKit Web Services_ framework provides  a JavaScript library (CloudKit JS) that can be incorporated in your website to provide access to the same CloudKit based data and content as your Xamarin.iOS app.

> [!IMPORTANT]
> Before you can access, present or update content from a CloudKit database using CloudKit JS, you must have previously defined that database's schema.

For more information, please see the following documents:

- [Introduction to CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) - Our introduction to using CloudKit in a Xamarin.iOS app.
- [CloudKit Quick Start](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) - Apple's introduction to CloudKit.
- [CloudKit JS Reference](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) - Apple's CloudKit JS documentation.
- [CloudKit Catalog: An Introduction to CloudKit (Cocoa and JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) - Apple's sample app using CloudKit and CloudKit JS.

> [!IMPORTANT]
> Apple [provides tools](https://developer.apple.com/support/allowing-users-to-manage-data/)
> to help developers properly handle the European Union's General Data
> Protection Regulation (GDPR).

## Foundation Framework Additions

Apple included the following changes to the Foundation framework in iOS 9:

### Changes to NSBundle

The following changes have been made to the [NSBundle](xref:Foundation.NSBundle) class for iOS 9:

- `GetPreservationPriorityForTag (NSString tag)` - Gets the current preservation priority for resources with the given tag. Valid values are in the range `0.0` to `1.0`, resources with the lowest priority will be purged first.
- `SetPreservationPriorityForTag (double priority, NSSet tags)` - Sets the current preservation priority for resources with the given tags. Valid values are in the range `0.0` to `1.0`, resources with the lowest priority will be purged first.

For more information, please see Apple's [NSBundle Reference](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### Changes to NSProcessInfo

Each process running on an iOS device has a single, _Process Information Agent_ (PIA). Use the [NSProcessInfo](xref:Foundation.NSProcessInfo) class to provide information about the current PIA and control power and thermal management for a given process.

For example, to control the automatic termination of a process you can use the following code:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

For more information, please see Apple's [NSProcessInfo Reference](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### Reacting to Low Power Mode

Use the `LowPowerModeEnabled` property of the [NSProcessInfo](xref:Foundation.NSProcessInfo) class to determine if the Low Power Mode has been enabled on the iOS device that the app is running on. For example:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## HealthKit Framework Changes

Apple included the following changes to the [HealthKit](xref:HealthKit) framework in iOS 9:

- Support for bulk deletion and deletion tracking of entries in the HealthKit database. See Apple's [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) and [HKHealthStore Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) for more information.
- New tracking categories and characteristics have been added to the `HKQuantityTypeIdentifier` class (such as `UVExposure`) and to the `HKCategoryTypeIdentifier` class (such as `OvulationTestResult`). 

Please see our [Introduction to HealthKit](~/ios/platform/healthkit.md) documentation for more information on working with HealthKit in Xamarin.iOS.

## Local Authentication Framework Changes

Apple included the following changes to the [Local Authentication](xref:LocalAuthentication) framework in iOS 9:

- Using the `EvaluateAccessControl` and `EvaluatePolicy` methods of the [LAContext](xref:LocalAuthentication.LAContext) class, you can now reuse Touch ID matches from previous successful unlocking attempts.
- The ability to get a list of currently registered fingers.
- Support for tracking when a finger is added or removed from authentication.
- The ability to use _Authentication Context_ in Keychain calls and support for evaluating Keychain Access control lists.
- The ability to cancel a user prompt from code.

For more information, see [Touch ID and Face ID with Xamarin.iOS](~/ios/platform/touch-id-face-id.md).

### LAContext Changes

The following changes have been made to the [LAContext](xref:LocalAuthentication.LAContext) class for iOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** - Returns the maximum amount of time that a touch ID authentication can be reused.
- **EvaluatedPolicyDomainState** - Gets or sets the state of an evaluated policy.
- **MaxBiometryFailures** - Has been depreciated in iOS 9.
- **TouchIdAuthenticationAllowableReuseDuration** Gets or sets the amount of time that a touch ID authentication can be reused.
- **EvaluateAccessControl** - Asynchronously evaluates an authentication policy.
- **Invalidate** - Invalidates a given touch ID authentication.
- **IsCredentialSet** - Returns `true` if the credentials are currently set.
- **SetCredentialType** Sets the given credential type.

Please see Apple's [LAContext Reference](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) for more details.

## MapKit Framework Changes

Apple included the following changes to the [MapKit](xref:MapKit) framework in iOS 9:

- MapKit now provides support for launching the Map app directly into transit directions and for querying transit Estimated Time of Arrival (ETA) using the [MKLaunchOptions](xref:MapKit.MKLaunchOptions) and [MKDirections](xref:MapKit.MKLaunchOptions) classes.
- The search results returned by MapKit and the [CLGeocoder](xref:CoreLocation.CLGeocoder) class can also provide the result's time zone.
- You can now fully customize Map Annotations presented by your iOS app using the `DetailCalloutAccessoryView` property of the [MKAnnotationView](xref:MapKit.MKAnnotationView) class.

Please see our [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) and [Walkthrough - Exploring annotations and overlays in MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) documentation for more information on working with Maps and Annotations in Xamarin.iOS and Apple's [CLGeocoder Reference](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) for more information.

## PassKit Framework Additions

Apple included the following changes to the [PassKit](xref:PassKit) framework in iOS 9:

- Apple Pay now supports both store debit and credit cards along with Discover cards. See the **Payment Networks** section of Apple's [PKPaymentRequest Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) for more information.
- From directly within a Xamarin.iOS app, you can now add payment networks and card issuers to Apple Pay. See Apple's [PKAddPaymentPassViewController Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) for more details.

Please see our [Introduction to PassKit](~/ios/platform/passkit.md) documentation for more information on working with PassKit in Xamarin.iOS.

## Safari Services Framework Additions

Apple included the following changes to the [Safari Services](xref:SafariServices) framework in iOS 9:

- You can now use the new [SFSafariViewController](xref:SafariServices.SFSafariViewController) class to display web content within a Xamarin.iOS app. It provides the ability to share website data and cookies with the Safari app and includes several of Safari's features (such as Reader and AutoFill). [SFSafariViewController](xref:SafariServices.SFSafariViewController) features a **Done** button that will return users to your app when they are finished viewing the web content.

Because the [SFSafariViewController](xref:SafariServices.SFSafariViewController) class is tailored for displaying a single page of web content, you should consider using it to replace any [WKWebKit](xref:WebKit.WKWebView) or [UIWebView](xref:UIKit.UIWebView) controls within your existing Xamarin.iOS apps.

### Displaying a Website

The code below is an example of calling a [SFSafariViewController](xref:SafariServices.SFSafariViewController) from inside another view controller:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## UIKit Framework Changes

Apple has included many enhancements to several elements of the [UIKit](xref:UIKit) framework for iOS 9. The following sections will detail those changes.

### 3D Touch Events

New to iOS 9 and the iPhone 6s and iPhone 6s Plus, 3D Touch adds pressure sensitive gestures to your iOS apps. As a result, if your app is running on iOS 9 (or greater) and the iOS device is capable of supporting 3D Touch, changes in pressure will cause the `TouchesMoved` event to be raised.

Because of this change in behavior, your iOS apps should be prepared for the `TouchesMoved` event to be invoked more often, even if the X/Y coordinates have not changed.

For more information, please see our [Introduction to 3D Touch](~/ios/platform/3d-touch.md) guide.

### Document Open-in-Place Functionality

By using either the `FinishedLaunching (application, launchOptions)` or `WillFinishLaunching (Application, launchOptions)` methods of the [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) class, you can now open a document and modify it in place (as opposed to working on a copy).

To support the new open-in-place functionality, add the `LSSupportsOpeningDocumentsInPlace` key to your Xamarin.iOS app's **Info.plist** file with a value of `YES`.

Please see Apple's [UIApplicationDelegate Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) for more details.

### Enhanced Touch Events

Apple has provided several enhancements to Touch Events in iOS 9. These include the ability to use Touch Prediction and to get access to intermediate touches between display refreshes.

Please see Apple's [Event Handling Guide for iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) for more details.

### Fetching Tailored Content

The new `NSDataAsset` class allows a Xamarin.iOS app to fetch content tailored to the memory and graphic capabilities of the iOS device that it is currently running on.

### New Layout Anchors

The new `NSLayoutAnchor` and `NSLayoutDimension` layout anchor classes work with the new anchor properties of the [UIView](xref:UIKit.UIView) class (such as `LeadingAnchor` and `WidthAnchor`) to make layout easier in iOS 9.

Please see our [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) documentation for more information on working with AutoLayout and Size Classes in a Xamarin.iOS app and Apple's [NSLayoutAnchor Reference](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [NSLayoutDimension Reference](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) and [UIView Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) for more information.

### New Readable Content Margins

The new `UILayoutGuide` class can be used to provide readable content margins and define the draw regions for content inside of a view. See Apple's [UILayoutGuide Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) for more information.

### Text Input in Notifications Modifications

The [UIUserNotificationAction](xref:UIKit.UIUserNotificationAction) class has a new `Behavior` property that can be used to support text input from notifications.

### UIApplicationDelegate Changes

While not formally deprecated by Apple, they suggest replacing all calls to the `FinishedLaunching (UIApplication application)` method of the [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) class with either the `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` or `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` methods.

Please see Apple's [UIApplicationDelegate Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) for more details.

### UIKit Dynamics Changes

Apple included the following changes to the UIKit Dynamics in iOS 9:

- Dynamics now provides support for non-rectangular collision boundaries.
- The new, customizable `UIFieldBehavior` class is used to support various field types.
- Additional attachment types have been added to the `UIAttachmentBehavior` class.

Please see Apple's [UIAttachment Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) for more details.

### UIPickerView and UIDatePicker Changes

Prior to iOS 9, the [UIPickerView](xref:UIKit.UIPickerView) and the [UIDatePicker](xref:UIKit.UIDatePicker) controls were non-resizable and would automatically resize to fill the width of their container (usually the width of the iOS device the app was running on).

In iOS 9, this automatic resizing no longer occurs and the controls will be rendered at a 320 point width on all iOS devices, regardless of screen size and orientation.

To correct this situation, use Auto Layout and Size Classes to pin the width of the control to the edges of the parent container (view) and specify the required height. Please see our [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) documentation for more information on working with Auto Layout and Size Classes in a Xamarin.iOS app.

### New UITextInputAssistantItem Class

Use the new `UITextInputAssistantItem` class to layout Bar Button Groups in a _Shortcut Bar_. The Shortcut Bar is a new area that is available in the soft keyboard to provide typing shortcuts.

## Related Links

- [iOS 9 Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [Introduction to iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [What's New in iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)