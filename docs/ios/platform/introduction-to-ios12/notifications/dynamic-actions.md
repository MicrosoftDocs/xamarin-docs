---
title: "Dynamic notification action buttons in Xamarin.iOS"
description: "With iOS 12, a notification content extension can add, remove, and update the action buttons displayed alongside a notification. This document describes how to use dynamic notification action buttons with Xamarin.iOS."
ms.service: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
no-loc: [Objective-C]
---
# Dynamic notification action buttons in Xamarin.iOS

In iOS 12, notifications can dynamically add, remove, and update their
associated action buttons. Such customization makes it possible to provide
users with actions that are directly relevant to the notification's content
and the user's interaction with it.

## Sample app: RedGreenNotifications

The code snippets in this guide come from the
[RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
sample app, which demonstrates how to use Xamarin.iOS to work with
notification action buttons in iOS 12.

This sample app sends two types of local notifications: red and green.
After having the app send a notification, use 3D Touch to view its
custom user interface. Then, use the notification's action buttons to
rotate the image it displays. As the image rotates, a **Reset rotation**
button appears and disappears as necessary.

Code snippets in this guide come from this sample app.

## Default action buttons

A notification's category determines its default action buttons.

Create and register notification categories while an application launches.
For example, in the [sample app](#sample-app-redgreennotifications), the
`FinishedLaunching` method of `AppDelegate` does the following:

- Defines one category for red notifications and another for green
notifications
- Registers these categories by calling the
[`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
method of `UNUserNotificationCenter`
- Attaches a single
[`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
to each category

The following sample code shows how this works:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

Based on this code, any notification whose
[`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
is "red-category" or "green-category" will, by default, show a
**Rotate 20°** action button.

## In-app handling of notification action buttons

`UNUserNotificationCenter` has a `Delegate` property of type
[`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate).

In the sample app, `AppDelegate` sets itself as the user notification
center's delegate in `FinishedLaunching`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

Then, `AppDelegate` implements
[`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
to handle action button taps:

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

This implementation of `DidReceiveNotificationResponse` does not
handle the notification's **Rotate 20°** action button. Instead, the
notification's content extension handles taps on this button. The next
section further discusses notification action button handling.

## Action buttons in the notification content extension

A notification content extension contains a view controller that
defines the custom interface for a notification.

This view controller can use the `GetNotificationActions` and
`SetNotificationActions` methods on its
[`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
property to access and modify the notification's action buttons.

In the sample app, the notification content extension's view controller
modifies the action buttons only when responding to a tap on an
already-existing action button.

> [!NOTE]
> A notification content extension can respond to an action button tap in
> its view controller's [`DidReceiveNotificationResponse`](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*)
> method, declared as part of
> [IUNNotificationContentExtension](xref:UserNotificationsUI.IUNNotificationContentExtension).
>
> Though it shares a name with the `DidReceiveNotificationResponse` method
> [described above](#in-app-handling-of-notification-action-buttons), this
> is a different method.
>
> After a notification content extension finishes processing a button tap,
> it can choose whether or not to tell the main application to handle that
> same button tap. To do this, it must pass an appropriate value of
> [UNNotificationContentExtensionResponseOption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption)
> to its completion handler:
>
> - `Dismiss`
> indicates that the notification interface should be dismissed, and that
> the main app does not need to handle the button tap.
> - `DismissAndForwardAction`
> indicates that the notification interface should be dismissed, and that
> the main app should also handle the button tap.
> - `DoNotDismiss`
> indicates that the notification interface should not be dismissed, and
> that the main app does not need to handle the button tap.

The content extension's `DidReceiveNotificationResponse` method determines
which action button was tapped, rotates the image in the notification's
interface, and shows or hides a **Reset** action button:

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

In this case, the method passes
`UNNotificationContentExtensionResponseOption.DoNotDismiss` to its
completion handler. This which means that the notification's interface will
stay open.

## Related links

- [Sample app – RedGreenNotifications](/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Declaring Your Actionable Notification Types](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)