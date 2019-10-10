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

For the Xamarin.Forms application to send and receive notifications on Android, the application must provide an implementation of the `INotificationManager`:

```csharp
using Android.Support.V4.App;
using Xamarin.Forms;
using AndroidApp = Android.App.Application;

[assembly: Dependency(typeof(LocalNotifications.Droid.AndroidNotificationManager))]
namespace LocalNotifications.Droid
{
    public class AndroidNotificationManager : INotificationManager
    {
        const string channelId = "default";
        const string channelName = "Default";
        const string channelDescription = "The default channel for notifications.";
        const int pendingIntentId = 0;

        public const string TitleKey = "title";
        public const string MessageKey = "message";

        bool channelInitialized = false;
        int messageId = -1;
        NotificationManager manager;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            CreateNotificationChannel();
        }

        public int ScheduleNotification(string title, string message)
        {
            if(!channelInitialized)
            {
                CreateNotificationChannel();
            }

            messageId++;

            Intent intent = new Intent(AndroidApp.Context, typeof(MainActivity));
            intent.PutExtra(TitleKey, title);
            intent.PutExtra(MessageKey, message);

            PendingIntent pendingIntent = PendingIntent.GetActivity(AndroidApp.Context, pendingIntentId, intent, PendingIntentFlags.OneShot);

            NotificationCompat.Builder builder = new NotificationCompat.Builder(AndroidApp.Context, channelId)
                .SetContentIntent(pendingIntent)
                .SetContentTitle(title)
                .SetContentText(message)
                .SetLargeIcon(BitmapFactory.DecodeResource(AndroidApp.Context.Resources, Resource.Drawable.xamagonBlue))
                .SetSmallIcon(Resource.Drawable.xamagonBlue)
                .SetDefaults((int)NotificationDefaults.Sound | (int)NotificationDefaults.Vibrate);

            var notification = builder.Build();
            manager.Notify(messageId, notification);

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message,
            };
            NotificationReceived?.Invoke(null, args);
        }

        void CreateNotificationChannel()
        {
            manager = (NotificationManager)AndroidApp.Context.GetSystemService(AndroidApp.NotificationService);

            if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
            {
                var channelNameJava = new Java.Lang.String(channelName);
                var channel = new NotificationChannel(channelId, channelNameJava, NotificationImportance.Default)
                {
                    Description = channelDescription
                };
                manager.CreateNotificationChannel(channel);
            }

            channelInitialized = true;
        }
    }
}
```

The `assembly` attribute above the namespace is critical to allow the `DependencyService` to register this implementation of the `INotificationManager` interface.

The Android OS allows applications to define multiple channels for notifications. The `Initialize` method creates a basic channel the sample application will use to send notifications. The `ScheduleNotification` method defines the platform-specific logic required to create and send a notification. Finally, the `ReceiveNotification` method will be called by the Android OS when a message is received, and invokes the event handler.

The `MainActivity` class must detect incoming notifications and notify the `AndroidNotificationManager`. The `MainActivity` class `Activity` attribute should specify a `LaunchMode` value of `LaunchMode.SingleTop`:

```csharp
[Activity(
        //...
        LaunchMode = LaunchMode.SingleTop]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

This mode prevents multiple instances of an `Activity` from being started as long as the application is in the foreground. For more information about `LaunchMode` values, see [Android Activity LaunchMode](https://developer.android.com/guide/topics/manifest/activity-element#lmode).

The rest of the `MainActivity` class is as follows:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);

            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());

            CreateNotificationFromIntent(Intent);
        }

        protected override void OnNewIntent(Intent intent)
        {
            CreateNotificationFromIntent(intent);
        }

        private void CreateNotificationFromIntent(Intent intent)
        {
            if(intent?.Extras != null)
            {
                var title = intent.Extras.GetString(AndroidNotificationManager.TitleKey);
                var message = intent.Extras.GetString(AndroidNotificationManager.MessageKey);

                DependencyService.Get<INotificationManager>().ReceiveNotification(title, message);
            }
        }
    }
```

The `CreateNotificationFromIntent` method extracts notification data from the `intent` argument and provides it to the `AndroidNotificationManager` using the `ReceiveNotification` method.

The `CreateNotificationFromIntent` method is called from both the `OnCreate` method and the `OnNewIntent` method:

- When the application is started by a notification data, the `Intent` data will be passed to the `OnCreate` method.
- If the application is already in the foreground, the `Intent` data will be passed to the `OnNewIntent` method.

Once this functionality is implemented, the Android implementation of the `INotificationManager` is capable of both sending and receiving notifications.

Android supports more functionality than is used here. For more information, see [Notifications in Xamarin.Android](~/xamarin/android/app-fundamentals/notifications/index.md).

## Create iOS interface implementation

## Related links

- Sample project
- Local Notifications on Android
- Local Notifications on iOS
- Xamarin.Forms DependencyService