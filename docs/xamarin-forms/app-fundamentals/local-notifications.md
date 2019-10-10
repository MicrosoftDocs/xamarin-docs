---
title: "Xamarin.Forms local notifications"
description: "This article explains how to handle local notifications in Xamarin.Forms."
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/10/2019
---

# Local notifications in Xamarin.Forms

Local notifications are alerts sent by applications installed on a mobile device. Local notifications are often used for features such as:

- Calendar events
- Reminders
- Location-based triggers

Each platform handles the creation, display, and consumption of local notifications differently. This article explains how to create a cross-platform abstraction to send and receive notifications with Xamarin.Forms.

## Create a cross-platform interface

The Xamarin.Forms application should be able to create and consume notifications without concern for the underlying platform implementations. The following `INotificationManager` interface defines an API that the application can use to interact with notifications:

```csharp
public interface INotificationManager
{
    event EventHandler NotificationReceived;

    void Initialize();

    int ScheduleNotification(string title, string message);

    void ReceiveNotification(string title, string message);
}
```

The interface provides a `NotificationReceived` event that the application can implement to handle incoming notifications. The `Initialize` method should perform any native platform logic needed to prepare the notification system. The `ScheduleNotification` method sends a notification. The `ReceiveNotification` method is called by the underlying platform when a message is received.


## Create Android interface implementation

## Create iOS interface implementation

## Related links