---
title: "Interactive notification user interfaces in Xamarin.iOS"
description: "With iOS 12, it is possible to create interactive user interfaces for local and remote notifications. This guide describes how to use these features with Xamarin.iOS."
ms.service: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
no-loc: [Objective-C]
---
# Interactive notification user interfaces in Xamarin.iOS

[Notification content extensions](~/ios/platform/user-notifications/advanced-user-notifications.md),
introduced in iOS 10, make it possible to create custom user interfaces
for notifications. Starting with iOS 12, notification user interfaces can
contain interactive elements such as buttons and sliders.

## Sample App: RedGreenNotifications

The [RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
sample app contains a notification content extension with an interactive
user interface.

Code snippets in this guide come from this sample.

## Notification content extension Info.plist file

In the sample app, the **Info.plist** file in the
**RedGreenNotificationsContentExtension** project contains the following
configuration:

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

Note the following features:

- The `UNNotificationExtensionCategory` array specifies the type of
notification categories the content extension handles.
- In order to support interactive content, the notification content
extension sets the `UNNotificationExtensionUserInteractionEnabled`
key to `true`.
- The `UNNotificationExtensionInitialContentSizeRatio` key specifies the
initial height/width ratio for the content extension's interface.

## Interactive interface

**MainInterface.storyboard**, which defines the interface for a
notification content extension, is a standard storyboard containing a single
view controller. In the sample app, the view controller is of type
`NotificationViewController`, and it contains an image view, three buttons,
and a slider. The storyboard associates these controls with handlers defined
in **NotificationViewController.cs**:

- The **Launch App** button handler calls the
`PerformNotificationDefaultAction` action method on `ExtensionContext`,
which launches the app:

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    In the app, the user notification center's `Delegate` (in the sample
    app, the `AppDelegate`) can respond to the interaction in the
    `DidReceiveNotificationResponse` method:

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- The **Dismiss Notification** button handler calls
`DismissNotificationContentExtension` on `ExtensionContext`, which closes
the notification:

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- The **Remove Notification** button handler dismisses the notification and
removes it from Notification Center:

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- The method that handles value changes on the slider updates the alpha of
the image displayed in the notification's interface:

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## Related links

- [Sample app – RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Rich Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)