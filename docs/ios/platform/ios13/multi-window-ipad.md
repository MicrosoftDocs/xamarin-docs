---
title: "Multiple Windows for iPad in Xamarin.iOS"
description: "Add multiple window support to your iPad applications."
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
---

# Multiple windows for iPad

iOS 13 now supports side-by-side windows for the same app on iPad. This enables new experiences and drag-and-drop interactions between windows. This document shows you how to setup your application to support this feature, and introduces these new features. 

## Project Configuration

To configure your project for multiple windows, amend the `info.plist` to declare `NSUserActivityTypes` telling iOS your app will handle activities of this type, and `UIApplicationSceneManifest` to enable `UIApplicationSupportsMultipleScenes` for multiple windows and `UISceneConfigurations` to associate your scene with a storyboard.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## Opening Multiple Windows

With your app open and running on an iPad, there are a few ways to open multiple windows of that app that iOS handles be default.

- **App Expose** - tap the app icon in the dock to enter this mode while your app is open.
- **Slide Over** - drag the app icon from the dock over the top of your running app to have a floating window.
- **Split Screen** - drag the app icon from the dock to the edge of the iPad screen to create a new side-by-side window.

Additional interactions to enter a multiple window mode are available from within your application.

**Drag from App** - use a drag interaction within your app to start a new `NSUserActivity` just like dragging your app icon in previous examples.

When using a [drag-and-drop interaction][0], you create an `NSUserActivity` and associate the data to be passed along to the new window you're asking iOS to open for you.

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

In the code above, the `selectedPhoto` model object has a method to return an `NSUserActivity` called `OpenDetailUserActivity()`. When the drag gesture is complete, the `UIDragItem` with present the `userActivity` via the `NSItemProvider`.

**Explicit Actions** - user gestures on buttons or links offer the ability to open a new window.

From the `UIApplication` you can start a new `UISceneSession` by calling `RequestSceneSessionActivation`. If an existing scene already exists, you should use that. By default a new scene will be created for you.

```csharp
public void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

In this example, the `userActivity` is the only value passed to the `RequestSceneSessionActivation` method in order to open a new window of the application based on an explicit user action; in this case an `ItemSelected` handler of a `UICollectionView`.

## Related links

- [Drag and Drop in Xamarin.iOS][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
