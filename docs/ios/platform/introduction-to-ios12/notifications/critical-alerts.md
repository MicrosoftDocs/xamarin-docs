---
title: "Critical alerts in Xamarin.iOS"
description: "This document describes how to use critical alerts with Xamarin.iOS. Critical alerts, introduced with iOS 12, are disruptive notifications that play a sound regardless of whether Do Not Disturb is on or the ringer switch is off."
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
---
# Critical alerts in Xamarin.iOS

With iOS 12, apps can send critical alerts. Critical alerts play a sound
regardless of whether or not Do Not Disturb is enabled or the ringer
switch is off. These notifications are disruptive and should only be used
when users must take immediate action.

## Custom critical alert entitlement

To display critical alerts in your app, first
[request a custom critical alert notifications entitlement](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/)
from Apple.

After receiving this entitlement from Apple and following any associated
instructions about how to configure your app to use it, add the custom
[entitlement](~/ios/deploy-test/provisioning/entitlements.md) to your app's
**Entitlements.plist** file(s). Then, configure your **iOS Bundle Signing**
options to use **Entitlements.plist** when signing the app on both simulator
and device.

## Request authorization

An app's notification authorization request prompts the user to allow or
disallow an app's notifications. If the notification authorization request
asks for permission to send critical alerts, the app will also give the user
a chance to opt in to critical alerts.

The following code requests permission to send both critical alerts and
standard notifications and sounds by passing the appropriate
[`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
values to
[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*):

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## Local critical alerts

To send a local critical alert, create a
[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
and set its `Sound` property to either:

- `UNNotificationSound.DefaultCriticalSound`, which uses the default
critical notification sound.
- `UNNotificationSound.GetCriticalSound`, which allows you to specify a
custom sound that is bundled with your app and a volume.

Then, create a `UNNotificationRequest` from the notification content and
add it to the notification center:

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> Critical alerts will not be delivered if they are not enabled for your
> app. Along with the prompt that appears the first time an app requests
> permission to send critical alerts, a user can also enable or disable
> critical alerts in your app's **Notifications** section of the iOS
> **Settings** app.

## Remote critical alerts

For information about remote critical alerts, please see the
[What's New In User Notifications](https://developer.apple.com/videos/play/wwdc2018/710/)
session from WWDC 2018 and the
[Generating a Remote Notification](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
document.

## Related links

- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
