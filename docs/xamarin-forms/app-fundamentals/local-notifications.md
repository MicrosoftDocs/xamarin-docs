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

## Implement notification interface in the application

Once an interface has been created, it can be implemented in the shared Xamarin.Forms project even though platform implementations haven't been created yet. The sample application contains a `ContentPage` called **MainPage.xaml** with the following content:

```xaml
<StackLayout Margin="0,35,0,0"
                x:Name="stackLayout">
    <Label Text="Click the button to create a local notification."
            TextColor="Red"
            HorizontalOptions="Center"
            VerticalOptions="Start" />
    <Button Text="Create Notification"
            HorizontalOptions="Center"
            VerticalOptions="Start"
            Clicked="OnScheduleClick"/>
</StackLayout>
```

The layout contains a 'Label' element with instructions for the user and a `Button` that should schedule a notification when tapped or clicked.

The `MainPage` class code-behind handles the sending and receiving of notifications:

```csharp
public partial class MainPage : ContentPage
{
    INotificationManager notificationManager;
    int notificationNumber = 0;
    public MainPage()
    {
        InitializeComponent();

        notificationManager = DependencyService.Get<INotificationManager>();
        notificationManager.NotificationReceived += (sender, eventArgs) =>
        {
            var evtData = (NotificationEventArgs)eventArgs;
            ShowNotification(evtData.Title, evtData.Message);
        };
    }

    private void OnScheduleClick(object sender, EventArgs e)
    {
        notificationNumber++;
        string title = $"Local Notification #{notificationNumber}";
        string message = $"You have now received {notificationNumber} notifications!";
        notificationManager.ScheduleNotification(title, message);
    }

    private void ShowNotification(string title, string message)
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            var msg = new Label()
            {
                Text = $"Notification Received:\nTitle: {title}\nMessage: {message}"
            };
            stackLayout.Children.Add(msg);
        });
    }
}
```

The `MainPage` class constructor uses the Xamarin.Forms `DependencyService` to retrieve a platform-specific instance of the `INotificationManager`. The `OnScheduleClicked` method uses the `INotificationManager` instance to schedule a new event. The `ShowNotification` method is called from the event handler attached to `NotificationReceived`, and will insert a new `Label` into the layout when the event is invoked.

For more information about the Xamarin.Forms Dependency Service, see [`Xamarin.Forms Dependency.Service`](~/xamarin-forms/app-fundamentals/dependency-service/introduction).

## Create Android interface implementation

## Create iOS interface implementation

## Related links