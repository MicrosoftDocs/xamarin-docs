---
title: "Introduction to iOS 10"
description: "This article introduces all of the new and modified APIs and features available in iOS 10 for Xamarin.iOS developers."
ms.service: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Introduction to iOS 10

With the new iOS 10 SDK, Apple has included new APIs and services that enable the developer to create new categories of apps and features. An iOS app can now extend the Messages, Siri, Phone and Maps apps to provide rich, engaging functionality to the end user that was previously unavailable.

For more information on iOS 10, please see Apple's [iOS + Apps](https://developer.apple.com/ios/) documentation.

## What's New in iOS 10

Apple has added several new APIs and services in iOS 10 along with many enhancements to existing features, including:

## Adapting to the True Tone Display

Apple's True Tone Display technology uses the ambient light sensor in an iOS device to dynamically adjust the color and intensity of the display to match the current lighting conditions. iOS 10 provides the new [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) key that can be added to the app's `Info.plist` file and controls how True Tone applies the standard color shift. 

The following values are available:

- `UIWhitePointAdaptivityStyleStandard` **Default** - Use the standard white-point adaptivity.
- `UIWhitePointAdaptivityStyleReading` - Used for reading-focused apps.
- `UIWhitePointAdaptivityStyleGame` - Used for game-focused apps.
- `UIWhitePointAdaptivityStyleVideo` - Used for video-focused apps.
- `UIWhitePointAdaptivityStylePhoto` - Used for photography-focused apps where color fidelity is more important than environmental white-point adjustments.

## App Extensions

Apple has provided several new App Extension Points in iOS 10:

- Call Directory
- Intents and Intents UI
- Messages
- Notification Content
- Notification Services
- Sticker Pack

Additionally, 3rd party Keyboard App Extensions have the following enhancements:

- The new `DocumentInputMode` property of the `UITextDocumentProxy` class can determine the input language of a document and allow the keyboard extension to align with that language.
- The new `HandleInputModeList` method lets the keyboard extension display the system's keyboard picker menu in response to the Globe Key being tapped.

For more information, please see our [Introduction to Extensions](~/ios/platform/extensions.md), [Message App Integration](~/ios/platform/message-app-integration/index.md), [Introduction to Proactive Suggestions](~/ios/platform/search/proactive-suggestions.md), [Introduction to SiriKit](~/ios/platform/sirikit/index.md), [Introduction to User Notifications](~/ios/platform/user-notifications/index.md) and Apple's [App Extension Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## App Search Enhancements

Core Spotlight in iOS 10 provides several enhancements to App Search such as:

- **Crowdsourced Deep-Link Popularity (with differential privacy)** - Provides a way to promote deep-linked app content in search results.
- **In-App Searching** - Use the new `CSSearchQuery` class to provide in-app Spotlight search ability similar to how the Mail, Messages and Notes apps work.
- **Search Continuation** - Allows a user to start a search in Spotlight or Safari, then open an app and continue that search.
- **Visualization of Validation Results** - Apple's [App Search API Validation Tool](https://search.developer.apple.com/appsearch-validation-tool) now displays a visual representation of a website's markup and deep-linking when preforming tests.
- **Message App Image Sharing** - Allows popular in-app images provided for sharing in Messages (via a Message App Extension) to appear in Spotlight searches.

To find out more, please see our [App Search Enhancements](~/ios/platform/search/app-search-enhancements.md) guide.

## Apple Pay Enhancements

Apple has made several enhancements to Apple Pay in iOS 10 that allow the user to make secure payments from websites and through interaction with Siri and Maps.

With iOS 10, several new APIs have been added that work with both iOS and watchOS to support dynamic payment networks and a new sandbox test environment.

Additionally, the PassKit framework has been expanded to support Apple Pay outside of `UIKit` and to allow card issuers to present their cards from within their apps.

To find out more, please see our [Apple Pay Enhancements](~/ios/platform/apple-pay.md) guide.

## Alternate App Icons

Apple has added several enhancements to iOS 10.3 that allow an app to manage its icon:

- `ApplicationIconBadgeNumber` - Gets or sets the badge of the app icon in the Springboard.
- `SupportsAlternateIcons` - If `true` the app has an alternate set of icons.
- `AlternateIconName` - Returns the name of the alternate icon currently selected or `null` if using the primary icon.
- `SetAlternameIconName` - Use this method to switch the app's icon to the given alternate icon.

To find out more, please see our [Alternate App Icons](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) guide.

## Introduction to CallKit

The new CallKit API in iOS 10 provides a way for VOIP apps to integrate with the iPhone UI and provide a familiar interface and experience to the end user. With this API, users can view and interact with VOIP calls from the iOS device's Lock Screen and manage contacts using the Phone app's **Favorites** and **Recents** views.

Additionally, the CallKit API provides the ability to create App Extensions that can associate a phone number with a name (Caller ID) or tell the system when a number should be blocked (Call Blocking).

To find out more, please see our [Introduction to Callkit](~/ios/platform/callkit.md) guide.

## Message App Integration

iOS 10 allows the inclusion of a Message App Extension in the Xamarin.iOS solution that integrates with the **Messages** app and presents new functionality to the user. The extension can send text, stickers, media files and interactive messages. Two types of Message App Extension are available:

- **Sticker Packs** - Contains a collection of stickers that the user can add to a message. Sticker Packs can be created without writing any code.
- **iMessage App** - Can present a custom User Interface within the Messages app for selecting stickers, entering text, including media files (with optional type conversions) and creating, editing and sending interaction messages.

To find out more, please see our [Message App Integration](~/ios/platform/message-app-integration/index.md) guide.

## News Publisher Enhancements

With iOS 10, Apple will allow anyone from major magazines and new organizations to bloggers and independent publishers to sign up and product and deliver content to the Apple News app. To learn more, please see Apple's [News Resources](https://newsresources.apple.com/) documentation.

## Providing Haptic Feedback

On the iPhone 7 and iPhone 7 Plus, Apple has included new haptics responses that provide additional ways to physically engage the user. Use the new tactile feedback options to get the user's attention and reinforce their actions.

Several built-in UI elements already provide haptic feedback such as Pickers, Switches and Sliders. iOS 10 now adds the ability to programmatically trigger haptics using a concrete subclass of the `UIFeedbackGenerator` class.

To find out more, please see our [Providing Haptic Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md) guide.

## Proactive Suggestions

iOS 10 presents new ways of driving engagement to an app by allowing the system to proactively present helpful information automatically to the user at appropriate times. Just as iOS 9 provided the ability to add deep search to the app using Spotlight, Handoff and Siri Suggestions, with iOS 10 an app can expose functionality that can be presented to the user by the system from within the following locations:

- The App Switcher
- The Lock Screen
- CarPlay
- Maps
- Siri Interactions
- QuickType Suggestions

An app exposes this functionality to the system using a collection of technologies such as [NSUserActivity](xref:Foundation.NSUserActivity), web markup, Core Spotlight, MapKit, Media Player and UIKit.

To find out more, please see our [Introduction to Proactive Suggestions](~/ios/platform/search/proactive-suggestions.md) guide.

## Request App Review

New to iOS 10.3, the `RequestReview()` method allows an iOS app to ask the user to rate or review it. While this method can be called at any point where it makes sense in the user experience, the review process is governed and handled by App Store policy. As a result, this method may or may not display an alert and should never be called in response to a user action, such as tapping a button.

To find out more, please see our [Request App Review](~/ios/platform/request-app-review.md) guide.

## Security and Privacy Enhancements

Apple has made several enhancements to both security and privacy in iOS 10 that will help the developer improve the security of their apps and ensure the end user's privacy.

As a result, apps running on iOS 10 (or later) must statically declare their intent to access specific features or user information by entering one or more Privacy Specific Keys in their `Info.plist` files that explain to the user why the app wishes to gain access.

To find out more, please see our [Security and Privacy Enhancements](~/ios/app-fundamentals/security-privacy.md) guide.

## SiriKit

New to iOS 10, SiriKit allows a Xamarin.iOS app to provide services that are accessible to the user using Siri on an iOS device. This functionality is provided in one or more App Extension using the new **Intents** and **Intents UI** frameworks.

SiriKit supports the following service domains:

- Audio or video calling.
- Booking a ride.
- Managing workouts.
- Messaging.
- Searching photos.
- Sending or receiving payments.

When the user makes a request of Siri involving one of the App Extension's services, SiriKit sends the extension an **Intent** object that describes the user's request along with any supporting data. The App Extension then generates the appropriate **Response** object for the given **Intent**, detailing how the extension can handle the request.

While Siri usually handles all user interaction, the App Extension can use the **Intent UI** framework to present a rich, custom User Interface featuring the app's branding and additional information.

To find out more, please see our [Introduction to SiriKit](~/ios/platform/sirikit/index.md) guide.

## Speech Recognition

iOS 10 includes a new Speech API that allows the app to support continuous speech recognition and transcribe speech (from live or recorded audio streams) into text.

Because speech recognition requires the transmission and temporary storage of data on Apple's servers, the app _must_ request the user's permission to perform recognition by including the `NSSpeechRecognitionUsageDescription` key in its `Info.plist` file and calling the `SFSpeechRecognizer.RequestAutorization` method.

To find out more, please see our [Introduction to Speech Recognition](~/ios/platform/speech.md) guide.

## User Notifications

New to iOS 10, the User Notification framework allows for the delivery and handling of local and remote notifications. Using this framework, the app or App Extension can schedule the delivery of local notifications by specifying a set of conditions such as location or time of day.

Additionally, the app or extension can receive (and potentially modify) both local and remote notifications as they are delivered to the user's iOS device.

The new User Notification UI framework allows the app or App Extension to customize the appearance of both local and remote notifications when they are presented to the user.

To find out more, please see our [User Notifications Framework](~/ios/platform/user-notifications/index.md) guide.

## Video Subscriber Account

New for iOS 10, the Video Subscriber Account framework allows apps that support authenticated streaming or video-on-demand to authenticate with their cable or satellite TV provider using a Single-Sign-in experience for the end user.

## Wide Color

iOS 10 extends the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

Additionally, [UIKit](xref:UIKit) has been modified to work in the new extended **sRGB** colorspace, making it easier to mix colors in wide color gamuts without significant performance loss.

Apple offers the following best practices when working with wide colors:

- [UIColor](xref:UIKit.UIColor) now uses the sRGB color space and will no longer clamp values to the `0.0` to `1.0` range. If the app relies on the previous clamp behavior, it will need to be modified for iOS 10.
- The drawing environment will be configured for the sRGB color space when performing custom `UIView` drawing on an iPad Pro.
- If the app performs custom rendering of `UIImages`, use the new [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) class to specify the use of the extended-range or standard-range formats.
- When using a low-level API such as Core Graphics or Metal to provide image processing, the developer should use an extended range color space and pixel format that supports 16-bit floating point values. Where necessary, the developer will have to manually clamp color component values.
- Core Graphics, Core Image and Metal Performance Shaders all provide new methods for converting between the two color spaces.

To find out more, please see our [Introduction to Wide Color](~/ios/platform/wide-color.md) guide.

## Widget Enhancements

Apple has introduced several enhancements to the Widget System to ensure that the widgets look great on any background that exists on the new iOS 10 Lock Screen. The [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) property has been deprecated and has been replaced with the new [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) or [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) properties. Additionally, widgets now contain a [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) property that allows the developer to describe how much content is available and allows the user to expand and collapse the content.

To find out more, please see our [Search and Home Screen Widget Enhancements](~/ios/platform/search/widgets.md) guide.

## Additional Framework Changes

In addition to the major framework changes and additions listed above, Apple has made many additional minor framework changes in iOS 10.

To find out more, please see our [Additional Framework Changes](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) guide.

## Deprecated APIs

The following APIs have been deprecated in iOS 10:

- The `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` and `CKFetchRecordChangesOperation` classes have been deprecated in CloudKit for iOS 10. Use the [CKDiscoverAllUserIdentitiesOperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation), [CKUserIdentity](xref:CloudKit.CKUserIdentity) and [CKFetchRecordZoneChangesOperation](xref:CloudKit.CKFetchRecordZoneChangesOperation) classes (which support record sharing) instead.
- Several [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) APIs (such as zone-based and query-based subscriptions) have been deprecated. Use the [CKRecordZoneSubscription](xref:CloudKit.CKRecordZoneSubscription) and [CKQuerySubscription](xref:CloudKit.CKQuerySubscription) APIs instead.
- [NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator) symbols related to ubiquitous content have been deprecated.
- `ADBannerView`, `ADInterstitialAd` and related symbols in the [UIViewController](xref:UIKit.UIViewController) class have been deprecated.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) symbols related to floating point values have been deprecated.
- The `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` and `UIUserNotificationSettings` classes of UIKit have been deprecated. Use the [User Notifications](#user-notifications) framework instead.
- The `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` and `DidReceiveRemoteNotification` WatchKit methods have been deprecated. Use the `HandleActionForNotification` and `DidReceiveNotification` methods instead.
- The `DidReceiveLocalNotification` and `DidReceiveRemoteNotification` methods of the [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) have been deprecated. Create an instance of [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) that implements the appropriate methods and assign it to the `Delegate` property of the [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) object.
- The **Game Center App** has been deprecated and removed from iOS. If the app uses GameKit, it _must_ present its own interface to display GameKit features such as leaderboards, etc.

See Apple's [iOS 9.3 to iOS 10.0 API Differences](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) documentation for a complete list of deprecations.

## Related Links

- [iOS 10 Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)