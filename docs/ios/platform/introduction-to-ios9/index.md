---
title: "Introduction to iOS 9"
description: "This article introduces all of the new and modified APIs and features available in iOS 9 for Xamarin.iOS developers."
ms.service: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
no-loc: [Objective-C]
---

# Introduction to iOS 9

_This article introduces all of the new and modified APIs and features available in iOS 9 for Xamarin.iOS developers._

![The iOS 9 logo](images/ios9-sml.png)

Apple has added several new APIs and services in iOS 9 along with many
enhancements to existing features.

## 3D Touch

New to iOS 9 and the iPhone 6s and iPhone 6s Plus, 3D Touch adds pressure sensitive gestures to your iOS apps. With 3D Touch, an iPhone app is now able to not only tell that the user is touching the device's screen, it can also sense how much pressure the user is exerting and respond to the different pressure levels.

3D Touch provides the following features to your app:

- **Pressure Sensitivity** - Apps can now measure how hard or light the user is touching the screen and take advantage of that information. For example, a painting app can make a line thicker or thinner based on how hard the user is touching the screen.
- **Peek and Pop** - Your app can now let the user interact with its data without having to navigate out of their current context. By pressing hard on the screen, they can *Peek* at the item they are interested in (like previewing a message). By pressing harder, they can *Pop* into the item.
- **Quick Actions** - Think of Quick Actions like the contextual menus that can be popped-up when a user right-clicks on an item in a desktop app. Using Quick Actions, you can add common, quick and easy to access shortcuts to functions in your app from the Home screen icon on the iOS device.

To find out more, please see our [Introduction to 3D Touch](~/ios/platform/3d-touch.md) guide.

## App Transport Security

New to iOS 9, App Transport Security (ATS) enforces secure connections between internet resources (such as the app's back-end server) and your app. ATS ensures that all internet communications conform to secure connection best practices, thereby preventing accidental disclosure of sensitive information either directly through your app or a library that it is consuming.

Since ATS is enabled by default in apps built for iOS 9 and OS X 10.11 (El Capitan), all connections using [NSUrlConnection](xref:Foundation.NSUrlConnection), [CFUrl](xref:CoreFoundation.CFUrl) or [NSUrlSession](xref:Foundation.NSUrlSession) will be subject to ATS security requirements. If your connections do not meet these requirement, they will fail with an exception.

To find out more about ATS, please see our [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

<a name="multitasking"></a>

## Multitasking for iPad

With iOS 9, Apple has added multitasking support for running two apps at the same time on specific iPad hardware. As a result, your Xamarin.iOS apps can no longer assume that they are the only app running at any given time or that they have access to the full screen or resources of the device.

Multitasking for iPad is supported via the following features:

- **Slide Over** - Allows the user to temporarily run a second iOS app in a slide out panel (either on the right or left side of the screen based on language direction) that covers approximately 25% of the main app currently running. Slide Over is available only on an iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3, or iPad Mini 4.
- **Split View** - On supported iPad hardware (iPad Air 2, iPad Mini 4 and iPad Pro only), the user can pick a second app and run it side-by-side with the currently running app in a split screen mode. The user can control the percentage of the main screen that each app occupies.
- **Picture in Picture** - For apps that playback video content, the video can now be played in a moveable and resizable window that floats over the other apps currently running on the iOS device. The user has full control over the size and position of this window. Picture in Picture is available only on an iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3, or iPad Mini 4.

To find out more about the new multitasking abilities of iOS 9, please see our [Multitasking for iPad](~/ios/platform/multitasking.md) guide.

## New Contacts and Contacts UI Frameworks

With the introduction of iOS 9, Apple has released two new frameworks, [Contacts](xref:Contacts) and [ContactsUI](xref:ContactsUI), that replace the existing Address Book and Address Book UI frameworks used by iOS 8 and prior.

These new, object-oriented frameworks provide the following:

- **Contacts** – Provides Xamarin.iOS access to the user's contact information. Because most apps only require read-only access, this framework has been optimized for thread safe, read-only access.
- **ContactsUI** – Provides Xamarin.iOS UI elements to display, edit, select and create contacts on iOS devices.

For more information, see our [Contacts and Contacts UI](~/ios/platform/contacts.md) documentation.

## New Search APIs

Search has been expanded in iOS 9 to provide great new ways to access information inside of your Xamarin.iOS app. Using the new Search APIs, you can make your app's content searchable through Spotlight and Safari search results, Handoff and Siri Reminders and Suggestions. This allows users quick access to activities and information deep within your app.

Additionally, the new Search APIs make it easier to integrate search in your app without prior search implementation experience. Because of this, Apple claims that it typically takes a few hours to make an iOS 9 app's content universally searchable using App Search.

For more information, please see our [Search Enhancements](~/ios/platform/search/index.md) documentation.

## New Stack View

The Stack View control ([UIStackView](xref:UIKit.UIStackView) leverages the power of Auto Layout and Size Classes to manage a stack of subviews (either horizontally or vertically) that dynamically responds to the iOS device's orientation and screen size.

By using Stack View control, the amount of work required to layout a user interface is greatly reduced. The layout of all subviews attached to a Stack View are managed automatically based on developer defined properties such as axis, distribution, alignment and spacing.

For more information, please see our [Introduction to Stack View](~/ios/user-interface/controls/uistackview.md) documentation.

## Collection View Changes

In iOS 9, the Collection View ([UICollectionView](xref:UIKit.UICollectionView) now supports drag reordering of items out of the box by adding a new default gesture recognizer and several new supporting methods.

Using these new methods, you can easily implement drag-to-reorder in your Collection View and have the option of customizing the items appearance during any stage of the reordering process.

To find out more about the Collection View changes for iOS 9, please see our [Collection View Changes](~/ios/user-interface/controls/uicollectionview.md) guide.

## Game Enhancements

With iOS 9, Apple has made several technological improvements to the Gaming APIs that make it easier to implement game graphics and audio in your Xamarin.iOS app. These include both ease of development through high-level frameworks and harnessing the power of the iOS device's GPU for improved speed and graphic abilities with low-level enhancements.

This includes GameplayKit, ReplayKit, Model I/O, MetalKit and Metal Performance Shaders along with new, enhanced features of Metal, SceneKit and SpriteKit.

For more information, please see our [Game Enhancements](~/ios/platform/gaming/index.md) documentation.

## HomeKit Framework Changes

The [HomeKit](xref:HomeKit) framework, introduced in iOS 8, provides the ability to setup and control various HomeKit enabled accessories (such as automated lights, door locks and garage door openers) from a Xamarin.iOS app. In addition to being easy to setup and configure, HomeKit accessories can be controlled via spoken Siri commands.

In iOS 9, Apple has made setup easier, expanded the types of accessories supported and provided more accessory interactions (such as controlling an accessory remotely via iCloud).

For more information, see our [Introduction to HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS Sample App](/samples/xamarin/ios-samples/homekit-homekitintro) and Apple's [HomeKit](https://developer.apple.com/homekit/) documentation.

## Handoff Framework Changes

Handoff (also known as Continuity) was introduced by Apple in iOS 8 and OS X Yosemite (10.10) as a way for the user to start an activity on one of their devices (either iOS or Mac) and continue that same activity on another of their devices (as identified by the user's iCloud Account).

Handoff was expanded in iOS 9 to also support new, enhanced Search capabilities. For more information, please see our [Search Enhancements](~/ios/platform/search/index.md) documentation. For more information on using Handoff, please see our [Introduction to Handoff](~/ios/platform/handoff.md) documentation.

## New Extension Points

In iOS 8, Apple introduced Extensions — libraries that are presented by the operating system in standard contexts, such as within the Notification Center, when the user requests a keyboard, or when they are editing a photo.

With iOS 9, Apple is extending Extension support by providing several new _Extension Points_ that define usage policies and provide APIs for working within a given area as follows:

- **New Audio Unit Extension Point** – Use this Extension Point to provide audio effects, musical instruments, sound generators, etc. for use within  other Audio Unit host apps (such as GarageBand). This Extension Point also allows you to sell _Audio Units_ (audio plug-ins) on the App Store.
- **New Index Maintenance Extension Point** — Use this Extension Point to support reindexing of app data without requiring an app relaunch.
- **New Network Extension Points** (these require special permission from Apple):
  - **App Proxy Provider Extension** — Use this Extension Point to implement a custom transparent client-side network proxy.
  - **Filter Data Provider / Filter Control Provider Extension** - Use these Extension Points to implement dynamic network content filtering on-device.
  - **Packet Tunnel Provider Extension** — Use this Extension Point to implement a custom VPN tunneling protocol client-side.
- **New Safari Extension Points**:
  - **Content Blocking Extension** — Use this Extension Point to define a list of blocked content that will not be displayed when the user is browsing the web.
  - **Shared Links Extension** — Use this Extension Point to enable viewing of your app's content in Safari's Shared Links.

For more information, please see our [Introduction to Extensions](~/ios/platform/extensions.md) and Apple's [App Extension Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) documentation.

## Keychain Enhancements

In iOS 9, Apple has enhanced the Keychain to provide a new encryption key type for the Secure Enclave and more item protection options as follows:

- A new Touch ID constraint that invalidates Keychain items when the fingerprint database is modified.
- New constraints that allow creating Access Control List entries with Touch ID or Passcode only.
- A new authentication context that allows you to invoke authentication separate from `SecItem` calls.
- Access Control List entropy (using the Application Password option) for app-provided keychain item encryption.
- Support for generating and using keys inside the secure enclave (via the `kSecAttrTokenIDSecureEnclave` attribute).

For more information, please see [Touch ID and Face ID in Xamarin.iOS](~/ios/platform/touch-id-face-id.md).

## Right-to-Left Language Support

In iOS 9, Apple has made presenting a flipped user interface easier than ever by providing full support for right-to-left languages. This includes the following:

- Standard [UIKit](xref:UIKit) controls will automatically flip right-to-left based on the iOS devices locale and language settings.
- The [UIView](xref:UIKit.UIView) class provides attributes that allow you to define how a given view should appear when flipped right-to-left.
- The ability to flip an image programmatically by using the [FlipsForRightToLeftLayoutDirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection) property of the [UIImage](xref:UIKit.UIImage) class.

For more information, please see Apple's [Supporting Right-to-Left Languages](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) documentation.

## Additional Framework Changes

In addition to the major changes that we have covered above, Apple has made modifications
and improvements to several existing frameworks for iOS 9 including the following:

- AV Foundation Framework
- AVKit Framework
- CloudKit Framework
- Foundation Framework
- Handoff Framework
- HealthKit Framework
- HomeKit Framework
- Local Authentication Framework
- MapKit Framework
- PassKit Framework
- Safari Services Framework
- UIKit Framework

For more information, please see our
[Additional iOS 9 Framework Changes](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) documentation.

## Deprecated APIs and Functions

Apple has deprecated the following APIs and functions in iOS 9:

- **Address Book & Address Book UI** - These APIs have been replaced by the Contact and Contact UI frameworks. For more information, see our [Contacts and Contacts UI](~/ios/platform/contacts.md) documentation.
- **CBCentralManager** - The `RetrievePeripherals` and `RetrieveConnectedPeripherals` methods of the `CBCentralManager` class have been removed in iOS 9. Calling these methods will cause an app to crash when pairing an accessory or on app launch.
- **FetchAllChanges** - The `FetchAllChanges` of the `CKFetchRecordChangesOperation` class was depreciated and will be removed in iOS 9.
- **Media Player** - The Media Player framework has been deprecated in iOS 9. Use either AVKit or AV Foundation APIs instead.

For a complete list of specific API deprecations, see Apple's
[iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) documentation.

## iOS 9 Sample Apps

We have some [iOS 9-specific samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9) to get started:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](/samples/xamarin/ios-samples/ios9-metalperformanceshadershelloworld)
- [MusicMotion](/samples/xamarin/ios-samples/ios9-musicmotion)
- [PhotoProgress](/samples/xamarin/ios-samples/ios9-photoprogress)
- [SegueCatalog](/samples/xamarin/ios-samples/ios9-seguecatalog)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Also check out the iOS portions of these samples (companion Mac OS X versions coming!):

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)

## Related Links

- [iOS 9 Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [Introduction to 3D Touch](~/ios/platform/3d-touch.md)
- [App Transport Security](~/ios/app-fundamentals/ats.md)
- [Multitasking for iPad](~/ios/platform/multitasking.md)
- [Contacts and Contacts UI](~/ios/platform/contacts.md)
- [New Search APIs](~/ios/platform/search/index.md)
- [Introduction to Stack View](~/ios/user-interface/controls/uistackview.md)
- [Collection View Changes](~/ios/user-interface/controls/uicollectionview.md)
- [Gaming Enhancements](~/ios/platform/gaming/index.md)
- [Introduction to HomeKit](~/ios/platform/homekit.md)
- [Introduction to Handoff](~/ios/platform/handoff.md)
- [Additional iOS 9 Framework Changes](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Troubleshooting](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [What's New in iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Updating your Xamarin.iOS apps to iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)