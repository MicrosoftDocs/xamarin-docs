---
title: "Xamarin.Forms local notifications"
description: "This article explains how to send and receive local notifications in Xamarin.Forms."
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/10/2019
---

# Local notifications in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)

Local notifications are alerts sent by applications installed on a mobile device. Local notifications are often used for features such as:

- Calendar events
- Reminders
- Location-based triggers

Each platform handles the creation, display, and consumption of local notifications differently. This article explains how to create a cross-platform abstraction to send and receive local notifications with Xamarin.Forms.

[![Local notifications application on iOS and Android](local-notifications-images/local-notifications-msg-cropped.png)](local-notifications-images/local-notifications-msg.png#lightbox)

## Create a cross-platform interface

The Xamarin.Forms application should create and consume notifications without concern for the underlying platform implementations. The following `INotificationManager` interface is implemented in the shared code library, and defines a cross-platform API that the application can use to interact with notifications:

```csharp
public interface INotificationManager
{
    event EventHandler NotificationReceived;

    void Initialize();

    int ScheduleNotification(string title, string message);

    void ReceiveNotification(string title, string message);
}
```

This interface will be implemented in each platform project. The `NotificationReceived` event allows the application to handle incoming notifications. The `Initialize` method should perform any native platform logic needed to prepare the notification system. The `ScheduleNotification` method should send a notification. The `ReceiveNotification` method should be called by the underlying platform when a message is received.

## Consume the interface in Xamarin.Forms

Once an interface has been created, it can be consumed in the shared Xamarin.Forms project even though platform implementations haven't been created yet. The sample application contains a `ContentPage` called **MainPage.xaml** with the following content:

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

The layout contains a `Label` element with instructions for the user and a `Button` that should schedule a notification when tapped.

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

    void OnScheduleClick(object sender, EventArgs e)
    {
        notificationNumber++;
        string title = $"Local Notification #{notificationNumber}";
        string message = $"You have now received {notificationNumber} notifications!";
        notificationManager.ScheduleNotification(title, message);
    }

    void ShowNotification(string title, string message)
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

The `MainPage` class constructor uses the Xamarin.Forms `DependencyService` to retrieve a platform-specific instance of the `INotificationManager`. The `OnScheduleClicked` method uses the `INotificationManager` instance to schedule a new notification. The `ShowNotification` method is called from the event handler attached to the `NotificationReceived` event, and will insert a new `Label` into the page when the event is invoked.

For more information about the Xamarin.Forms `DependencyService`, see [Xamarin.Forms DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md).

## Create the Android interface implementation

For the Xamarin.Forms application to send and receive notifications on Android, the application must provide an implementation of the `INotificationManager` interface.

### Create the AndroidNotificationManager class

The `AndroidNotificationManager` class implements the `INotificationManager` interface:

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
            if (!channelInitialized)
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

The `assembly` attribute above the namespace registers the `INotificationManager` interface implementation with the `DependencyService`.

Android allows applications to define multiple channels for notifications. The `Initialize` method creates a basic channel the sample application uses to send notifications. The `ScheduleNotification` method defines the platform-specific logic required to create and send a notification. Finally, the `ReceiveNotification` method is called by the Android OS when a message is received, and invokes the event handler.

> [!NOTE]
> The `Application` class is defined in both the `Xamarin.Forms` and `Android.App` namespaces so the `AndroidApp` alias is defined in the `using` statements to differentiate the two.

### Handle incoming notifications on Android

The `MainActivity` class must detect incoming notifications and notify the `AndroidNotificationManager` instance. The `Activity` attribute on the `MainActivity` class should specify a `LaunchMode` value of `LaunchMode.SingleTop`:

```csharp
[Activity(
        //...
        LaunchMode = LaunchMode.SingleTop]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

The `SingleTop` mode prevents multiple instances of an `Activity` from being started while the application is in the foreground. This `LaunchMode` may not be appropriate for applications that launch multiple activities in more complex notification scenarios. For more information about `LaunchMode` enumeration values, see [Android Activity LaunchMode](https://developer.android.com/guide/topics/manifest/activity-element#lmode).

In the `MainActivity` class is modified to receive incoming notifications:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    // ...

    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
    CreateNotificationFromIntent(Intent);
}

protected override void OnNewIntent(Intent intent)
{
    CreateNotificationFromIntent(intent);
}

void CreateNotificationFromIntent(Intent intent)
{
    if (intent?.Extras != null)
    {
        string title = intent.Extras.GetString(AndroidNotificationManager.TitleKey);
        string message = intent.Extras.GetString(AndroidNotificationManager.MessageKey);
        DependencyService.Get<INotificationManager>().ReceiveNotification(title, message);
    }
}
```

The `CreateNotificationFromIntent` method extracts notification data from the `intent` argument and provides it to the `AndroidNotificationManager` using the `ReceiveNotification` method. The `CreateNotificationFromIntent` method is called from both the `OnCreate` method and the `OnNewIntent` method:

- When the application is started by notification data, the `Intent` data will be passed to the `OnCreate` method.
- If the application is already in the foreground, the `Intent` data will be passed to the `OnNewIntent` method.

Android offers many advanced options for notifications. For more information, see [Notifications in Xamarin.Android](~/android/app-fundamentals/notifications/index.md).

## Create the iOS interface implementation

For the Xamarin.Forms application to send and receive notifications on iOS, the application must provide an implementation of the `INotificationManager`.

### Create the iOSNotificationManager class

The `iOSNotificationManager` class implements the `INotificationManager` interface:

```csharp
[assembly: Dependency(typeof(LocalNotifications.iOS.iOSNotificationManager))]
namespace LocalNotifications.iOS
{
    public class iOSNotificationManager : INotificationManager
    {
        int messageId = -1;

        bool hasNotificationsPermission;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            // request the permission to use local notifications
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert, (approved, err) =>
            {
                hasNotificationsPermission = approved;
            });
        }

        public int ScheduleNotification(string title, string message)
        {
            // EARLY OUT: app doesn't have permissions
            if(!hasNotificationsPermission)
            {
                return -1;
            }

            messageId++;

            var content = new UNMutableNotificationContent()
            {
                Title = title,
                Subtitle = "",
                Body = message,
                Badge = 1
            };

            // Local notifications can be time or location based
            // Create a time-based trigger, interval is in seconds and must be greater than 0
            var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger(0.25, false);

            var request = UNNotificationRequest.FromIdentifier(messageId.ToString(), content, trigger);
            UNUserNotificationCenter.Current.AddNotificationRequest(request, (err) =>
            {
                if (err != null)
                {
                    throw new Exception($"Failed to schedule notification: {err}");
                }
            });

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message
            };
            NotificationReceived?.Invoke(null, args);
        }
    }
}
```

The `assembly` attribute above the namespace registers the `INotificationManager` interface implementation with the `DependencyService`.

On iOS, you must request permission to use notifications before attempting to schedule a notification. The `Initialize` method requests authorization to use local notifications. The `ScheduleNotification` method defines the logic required to create and send a notification. Finally, the `ReceiveNotification` method will be called by iOS when a message is received, and invokes the event handler.

### Handle incoming notifications on iOS

On iOS, you must create a delegate that subclasses `UNUserNotificationCenterDelegate` to handle incoming messages. The sample application defines an `iOSNotificationReceiver` class:

```csharp
public class iOSNotificationReceiver : UNUserNotificationCenterDelegate
{
    public override void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
    {
        DependencyService.Get<INotificationManager>().ReceiveNotification(notification.Request.Content.Title, notification.Request.Content.Body);

        // alerts are always shown for demonstration but this can be set to "None"
        // to avoid showing alerts if the app is in the foreground
        completionHandler(UNNotificationPresentationOptions.Alert);
    }
}
```

This class uses the `DependencyService` to get an instance of the `iOSNotificationManager` class and provides incoming notification data to the `ReceiveNotification` method.

The `AppDelegate` class must specify the custom delegate during application startup. The `AppDelegate` class must specify an `iOSNotificationReceiver` object as the `UNUserNotificationCenter` delegate during application startup. This occurs in the `FinishedLaunching` method:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();

    UNUserNotificationCenter.Current.Delegate = new iOSNotificationReceiver();

    LoadApplication(new App());
    return base.FinishedLaunching(app, options);
}
```

iOS offers many advanced options for notifications. For more information, see [Notifications in Xamarin.iOS](~/ios/platform/user-notifications/index.md).

## Test the application

Once the platform projects contain a registered implementation of the `INotificationManager` interface, the application can be tested on both platforms. Run the application and click the **Schedule Notification** button to create a notification.

On Android, the notification will appear in the notification area. When the notification is tapped, the application receives the notification and displays a message below the **Schedule Notification** button:

![Local notifications on Android](local-notifications-images/local-notifications-android.png)

On iOS, incoming notifications are automatically received by the application without requiring user input. The application receives the notification and displays a message below the **Schedule Notification** button:

![Local notifications on iOS](local-notifications-images/local-notifications-ios.png)

## Related links

- [Sample project](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)
- [Notifications in Xamarin.Android](~/android/app-fundamentals/notifications/index.md)
- [Notifications in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Xamarin.Forms Dependency.Service](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md)
