---
title: "Broadcast Receivers in Xamarin.Android"
description: "This section discusses how to use a Broadcast Receiver."
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
---

# Broadcast Receivers in Xamarin.Android

_This section discusses how to use a Broadcast Receiver._

## Broadcast Receiver Overview

A _broadcast receiver_ is an Android component that allows an application to respond to messages (an Android [`Intent`](xref:Android.Content.Intent)) that are broadcast by the Android operating system or by an application. Broadcasts follow a _publish-subscribe_ model &ndash; an event causes a broadcast to be published and received by those components that are interested in the event.

Android identifies two types of broadcasts:

- **Explicit broadcast** &ndash; These types of broadcasts target a specific application. The most common use of an explicit broadcast is to start an Activity. An example of an explicit broadcast when an app needs to dial a phone number; it will dispatch an Intent that targets the Phone app on Android and pass along the phone number to be dialed. Android will then route the intent to the Phone app.
- **Implicit broadcast** &ndash; These broadcasts are dispatched to all apps on the device. An example of an implicit broadcast is the `ACTION_POWER_CONNECTED` intent. This intent is published each time Android detects that the battery on the device is charging. Android will route this intent to all apps that have registered for this event.

The broadcast receiver is a subclass of the `BroadcastReceiver` type and it must override the [`OnReceive`](xref:Android.Content.BroadcastReceiver.OnReceive*) method. Android will execute `OnReceive` on the main thread, so this method should be designed to execute quickly. Care should be taken when spawning threads in `OnReceive` because Android may terminate the process when the method finishes. If a broadcast receiver must perform long running work then it is recommended to schedule a _job_ using the `JobScheduler` or the _Firebase Job Dispatcher_. Scheduling work with a job will be discussed in a separate guide.

An _intent filter_ is used to register a broadcast receiver so that Android can properly route messages. The intent filter can be specified at runtime (this is sometimes referred to as a _context-registered receiver_ or as _dynamic registration_) or it can be statically defined in the Android Manifest (a _manifest-registered receiver_). Xamarin.Android provides a C# attribute, `IntentFilterAttribute`, that will statically register the intent filter (this will be discussed in more detail later in this guide). Starting in Android 8.0, it is not possible for an application to statically register for an implicit broadcast.

The primary difference between the manifest-registered receiver and the context-registered receiver is that a context-registered receiver will only respond to broadcasts while an application is running, while a manifest-registered receiver can respond to broadcasts even though the app may not be running.  

There are two sets of APIs for managing a broadcast receiver and sending broadcasts:

1. **`Context`** &ndash; The `Android.Content.Context` class can be used to register a broadcast receiver that will respond to system-wide events. The `Context` is also used to publish system-wide broadcasts.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; This is an API that is available through the [Xamarin Support Library v4 NuGet package](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). This class is used to keep broadcasts and broadcast receivers isolated in the context of the application that is using them. This class can be useful for preventing other applications from responding to application-only broadcasts or sending messages to private receivers.

A broadcast receiver may not display dialogs, and it is strongly discouraged to start an activity from within a broadcast receiver. If a broadcast receiver must notify the user, then it should publish a notification.

It is not possible to bind to or start a service from within a broadcast receiver.

This guide will cover how to create a broadcast receiver and how to register it so that it may receive broadcasts.

## Creating a Broadcast Receiver

To create a broadcast receiver in Xamarin.Android, an application should subclass the `BroadcastReceiver` class, adorn it with the `BroadcastReceiverAttribute`, and override the `OnReceive` method:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

When Xamarin.Android compiles the class, it will also update the AndroidManifest with the necessary meta-data to register the receiver. For a statically-registered broadcast receiver, the `Enabled` properly must be set to `true`, otherwise Android will not be able to create an instance of the receiver.

The `Exported` property controls whether the broadcast receiver can receive messages from outside the application. If the property is not explicitly set, the default value of the property is determined by Android based on if there are any intent-filters associated with the broadcast receiver. If there is at least one intent-filter for the broadcast receiver then Android will assume that the `Exported` property is `true`. If there are no intent-filters associated with the broadcast receiver, then Android will assume that the value is `false`.

The `OnReceive` method receives a reference to the `Intent` that was dispatched to the broadcast receiver. This makes it possible for the sender of the intent to pass values to the broadcast receiver.

### Statically registering a Broadcast Receiver with an Intent Filter

When a `BroadcastReceiver` is decorated with the [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute), Xamarin.Android will add the necessary `<intent-filter>` element to the Android manifest at compile time. The following snippet is an example of a broadcast receiver that will run when a device has finished booting (if the appropriate Android permissions were granted by the user):

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

> [!NOTE]
> In Android 8.0 (API 26 and above), [Google placed limitations](https://developer.android.com/about/versions/oreo/background) on what apps can do while users aren't directly interacting with them. These limitations affect background services and implicit broadcast receivers such as `Android.Content.Intent.ActionBootCompleted`. Because of these limitations, you might have difficulties registering a `Boot Completed` broadcast receiver on newer versions of Android. If this is the case, note that these restrictions do not apply to foreground services, which can be called from your broadcast receiver.

It is also possible to create an intent filter that will respond to custom intents. Consider the following example:

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Apps that target Android 8.0 (API level 26) or higher may not statically register for an implicit broadcast. Apps may still statically register for an explicit broadcast. There is a small list of implicit broadcasts that are exempt from this restriction. These exceptions are described in the [Implicit Broadcast Exceptions](https://developer.android.com/guide/components/broadcast-exceptions.html) guide in the Android documentation. Apps that are interested in implicit broadcasts must do so dynamically using the `RegisterReceiver` method. This is described next.

### Context-Registering a Broadcast Receiver

Context-registration  (also referred to as dynamic registration) of a receiver is performed by invoking the `RegisterReceiver` method, and the broadcast receiver must be unregistered with a call to the `UnregisterReceiver` method. To prevent leaking resources, it is important to unregister the receiver when it is no longer relevant for the context (the Activity or service). For example, a service may broadcast an intent to inform an Activity that updates are available to be displayed to the user. When the Activity starts, it would register for those Intents. When the Activity is moved into the background and no longer visible to the user, it should unregister the receiver because the UI for displaying the updates is no longer visible. The following code snippet is an example of how to register and unregister a broadcast receiver in the context of an Activity:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver();

        // Code omitted for clarity
    }

    protected override void OnResume()
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override void OnPause()
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

In the previous example, when the Activity comes into the foreground, it will register a broadcast receiver that will listen for a custom intent by using the `OnResume` lifecycle method. As the Activity moves into the background, the `OnPause()` method will unregister the receiver.

## Publishing a Broadcast

A broadcast may be published to all apps installed on the device creating an Intent object and dispatching it with the `SendBroadcast` or the `SendOrderedBroadcast` method.  

1. **Context.SendBroadcast methods** &ndash; There are several implementations of this method.
   These methods will broadcast the intent to the entire system. Broadcast receivers that will receive the intent in an indeterminate order. This
   provides a great deal of flexibility but means that it is possible for other applications to register and receive the intent. This can pose
   a potential security risk. Applications may need to implement
   addition security to prevent unauthorized access. One possible solution is to use the `LocalBroadcastManager` which will only dispatch messages within the private space of the app. This
   code snippet is one example of how to dispatch an intent using one
   of the `SendBroadcast` methods:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    This snippet is another example of sending a broadcast by using the
    `Intent.SetAction` method to identify the action:

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; This is method is very similar to `Context.SendBroadcast`, with the difference being that the intent will be published one at time to receivers, in the order that the receivers were registered.

### LocalBroadcastManager

The [Xamarin Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) provides a helper class called [`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). The `LocalBroadcastManager` is intended for apps that do not want to send or receive broadcasts from other apps on the device. The `LocalBroadcastManager` will only publish messages within the context of the application, and only to those broadcast receivers that are registered with the `LocalBroadcastManager`. This code snippet is an example of registering a broadcast receiver with `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Other apps on the device cannot receive the messages that are published with the `LocalBroadcastManager`. This code snippet shows how to dispatch an Intent using the `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## Related Links

- [BroadcastReceiver API](xref:Android.Content.BroadcastReceiver)
- [Context.RegisterReceiver API](xref:Android.Content.Context.RegisterReceiver*)
- [Context.SendBroadcast API](xref:Android.Content.Context.SendBroadcast*)
- [Context.UnregisterReceiver API](xref:Android.Content.Context.UnregisterReceiver*)
- [Intent API](xref:Android.Content.Intent)
- [IntentFilter API](xref:Android.App.IntentFilterAttribute)
- [LocalBroadcastManager (Android docs)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Local Notifications in Android](~/android/app-fundamentals/notifications/local-notifications.md) guide
- [Android Support Library v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
