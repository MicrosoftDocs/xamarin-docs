---
title: "Notification management in Xamarin.iOS"
description: "This document describes how to use Xamarin.iOS to take advantage of new notification management features introduced in iOS 12."
ms.service: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
no-loc: [Objective-C]
---
# Notification management in Xamarin.iOS

In iOS 12, the operating system can deep link from Notification Center
and the Settings app to an app's notification management screen. This
screen should allow users to opt in and out of the various types of
notifications the app sends.

## Sample app: RedGreenNotifications

To see an example of how notification management works, take a look at the
[RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
sample app.

This sample app sends two types of notifications – red and green – and
provides a screen that allows users to opt in or out of either kind.

Code snippets in this guide come from this sample app.

## Notification management screen

In the sample app, `ManageNotificationsViewController` defines a user
interface that allows users to independently enable and disable red
notifications and green notifications. It is a standard
[`UIViewController`](xref:UIKit.UIViewController)
containing a
[`UISwitch`](xref:UIKit.UISwitch) for
each notification type. Toggling the switch for either type of
notification saves, in user defaults, the user's preference for that
type of notification:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> The notification management screen also checks whether or not the user
> has completely disabled notifications for the app. If so, it hides
> the toggles for the individual notification types. To do this, the
> notification management screen:
>
> - Calls [`UNUserNotificationCenter.Current.GetNotificationSettingsAsync`](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync)
> and examines the [`AuthorizationStatus`](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus)
> property.
> - Hides the toggles for the individual notification types if notifications
> have been completely disabled for the app.
> - Re-checks whether notifications have been disabled each time the
> application moves to the foreground, since the user can enable/disable
> notifications in iOS Settings at any time.

The sample app's `ViewController` class, which sends the notifications,
check's the user's preference before sending a local notification to
ensure that the notification is of a type the user actually wants to
receive:

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## Deep link

iOS deep links to an app's notification management screen from
Notification Center and the app's notification settings in the Settings
app. To facilitate this, an app must:

- Indicate that a notification management screen is available by passing
`UNAuthorizationOptions.ProvidesAppNotificationSettings` to the app's
notification authorization request.
- Implement the `OpenSettings` method from
[`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate).

### Authorization request

To indicate to the operating system that a notification management screen
is available, an app should pass the
`UNAuthorizationOptions.ProvidesAppNotificationSettings` option (along
with any other notification delivery options it needs) to the
`RequestAuthorization` method on the `UNUserNotificationCenter`.

For example, in the sample app's `AppDelegate`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### OpenSettings method

The `OpenSettings` method, called by the system to deep link to an app's
notification management screen, should navigate the user directly to that
screen.

In the sample app, this method performs the segue to the
`ManageNotificationsViewController` if necessary:

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## Related links

- [Sample app – RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)