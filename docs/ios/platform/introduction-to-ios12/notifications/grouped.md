---
title: "Grouped notifications in Xamarin.iOS"
description: "With iOS 12, it is possible to group notifications in Notification Center or the lock screen by application or by thread. This document describes how to send threaded and unthreaded notifications with Xamarin.iOS."
ms.service: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
no-loc: [Objective-C]
---
# Grouped notifications in Xamarin.iOS

By default, iOS 12 places all of an app's notifications in a group. The
lock screen and Notification Center display this group as a stack with
the most recent notification on top. Users can expand the group to see all
the notifications it contains and dismiss the group as a whole.

Apps can also group notifications by thread, making it easier for users
to find and interact with the specific information they're interested in.

## Sample app: GroupedNotifications

To learn how to use grouped notifications with Xamarin.iOS, take
a look at the [GroupedNotifications](/samples/xamarin/ios-samples/ios12-groupednotifications)
sample app.

This sample app simulates conversations with various friends, sending a
notification for each message and grouping them by thread. It also
demonstrates how unthreaded notifications land in an application-level
group.

Code snippets in this guide come from this sample app.

## Request authorization and allow foreground notifications

Before an app can send local notifications, it must request
permission to do so. In the sample app's
[`AppDelegate`](xref:UIKit.UIApplicationDelegate),
the [`FinishedLaunching`](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary))
method requests this permission:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

The [`Delegate`](xref:UserNotifications.UNUserNotificationCenter.Delegate)
(set above) for a [`UNUserNotificationCenter`](xref:UserNotifications.UNUserNotificationCenter)
decides whether or not a foreground app should display an incoming
notification by calling the completion handler passed to
[`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions})):

```csharp
[Export("userNotificationCenter:willPresentNotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

The [`UNNotificationPresentationOptions.Alert`](xref:UserNotifications.UNNotificationPresentationOptions)
parameter indicates that the app should show the alert but not play a sound
or update a badge.

## Threaded notifications

Tap the sample app's **Message with Alice** button repeatedly to have it
send notifications for a conversation with a friend named Alice.
Since this conversation's notifications are part of the same thread, the
lock screen and Notification Center group them together.

To start a conversation with a different friend, tap the
**Choose a new friend** button. Notifications for this conversation appear
in a separate group.

### ThreadIdentifier

Any time the sample app starts a new thread, it creates a unique thread
identifier:

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

To send a threaded notification, the sample app:

- Checks whether the app has authorization to send a notification.
- Creates a
[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
object for the notification's content and sets its
[`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
to the thread identifier created above.
- Creates a request and schedules the notification:

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

All notifications from the same app with the same thread identifier will
appear in the same notification group.

> [!NOTE]
> To set a thread identifier on a remote notification, add the `thread-id`
> key to the notification's JSON payload. See Apple's
> [Generating a Remote Notification](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
> document for more details.

### SummaryArgument

`SummaryArgument` specifies how a notification will impact the summary
text that appears on the lower-left corner of a notification group to which
the notification belongs. iOS aggregates summary text from notifications
in the same group to create an overall summary description.

The sample app uses the message's author as the summary argument. Using
this approach, the summary text for a group of six notifications
with Alice might be **5 more notifications from Alice and Me**.

## Unthreaded notifications

Each tap of the sample app's **Appointment reminder** button sends one of
various appointment reminder notifications. Since these reminders are
not threaded, they appear in the application-level notification group on the
lock screen and in Notification Center.

To send an unthreaded notification, the sample app's
`ScheduleUnthreadedNotification` method uses similar code as above.
However, it does not set the `ThreadIdentifier` on the
`UNMutableNotificationContent` object.

## Related links

- [Sample app – GroupedNotifications](/samples/xamarin/ios-samples/ios12-groupednotifications)
- [User Notifications framework in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [What's New in User Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Using Grouped Notifications (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [Best Practices and What's New in User Notifications (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generating a Remote Notification (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)