---
title: "Provisional notifications in Xamarin.iOS"
description: "This document describes how to use Xamarin.iOS to work with provisional notifications. Provisional notifications, introduced in iOS 12, allow applications to send quiet notifications without explicit user permission."
ms.service: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
no-loc: [Objective-C]
---
# Provisional notifications in Xamarin.iOS

Provisional notifications allow apps to deliver notifications without a
user's explicit up-front consent. These notifications arrive quietly and
show only in Notification Center, which lets users preview them before
opting in or out of their continued delivery.

In Notification Center, users can specify that an app should stop
delivering provisional notifications, continue delivering them
provisionally, or start delivering them more prominently.

## Sample app: RedGreenNotifications

Take a look at the [RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
 sample app, which sends provisional notifications.

## Sending provisional notifications

To send provisional notifications, provide
`UNAuthorizationOptions.Provisional` as an option to the
[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
method of `UNUserNotificationCenter`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

If the user promotes provisional notifications to prominent delivery, the
`UNAuthorizationOptions` values passed to `RequestAuthorization`
will determine the new notification delivery settings (in the above code,
`UNAuthorizationOptions.Alert` and `UNAuthorizationOptions.Sound`).

## Related links

- [Sample app – RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)