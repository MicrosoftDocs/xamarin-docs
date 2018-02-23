---
title: "Foreground Services"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# Foreground Services

Some services are performing some tasks that users are actively  aware of, these services are known as _foreground services_. An example of a foreground service is an app that is providing the user with directions while driving or walking. Even if the app is in the background, it is still important that the service has sufficient resources to work properly and that the user has a quick and handy way to access the app. For an Android app, this means that a foreground service should receive higher priority than a "regular" service and a foreground service must provide a `Notification` that Android will display as long as the service is running.
 
A foreground service is created and started just as any other service. When the service is starting up, it will register itself with Android as a foreground service.
 
This guide will discuss the extra steps that must be taken to register a foreground service, and to stop the service when  it's done.

## Registering as a Foreground Service

A foreground service is a special type of a bound service or a started service. The service, once it has started, calls the [`StartForeground`](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) method to register itself with Android as a foreground service.   

`StartForeground` takes two parameters, both of which are mandatory:
 
* An integer value that is unique within the application to identify the service.
* A `Notification` object that Android will display in the status bar for as long as the service is running.

Android will display the notification in the status bar for as long as the service is running. The notification, at minimum, will provide a visual cue to the user that the service is running. Ideally, the notification should provide the user with a shortcut to the application or possibly some action buttons to control the application. An example of this is a music player &ndash; the notification that is displayed may have buttons to pause/play music, to rewind to the previous song, or to skip to the next song. 

This code snippet is an example of registering a service as a foreground service:   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

The previous notification will display a status bar notification that that is similar to the following:

![Image showing the notification in the status bar](foreground-services-images/foreground-services-01.png "Image showing the notification in the status bar")

This screenshot shows the expanded notification in the notification tray with two actions that allow the user to control the service:

![Image showing the expanded notification](foreground-services-images/foreground-services-02.png "Image showing the expanded notification.")

More information about notifications is available in the [Local Notifications](~/android/app-fundamentals/notifications/local-notifications.md) section of the [Android Notifications](~/android/app-fundamentals/notifications/index.md) guide.

## Unregistering as a Foreground Service

A service can de-list itself as a foreground service by calling the method `StopForeground`. `StopForeground` will not stop the service, but it will remove the notification icon and signals Android that this service can be shut down if necessary.

The status bar notification that is displayed can also be removed by passing `true` to the method: 

```csharp
StopForeground(true);
```

If the service is halted with a call to `StopSelf` or `StopService`, then the status bar notificaiton will likewise be removed.


## Related Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Local Notifications](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
