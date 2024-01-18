---
title: "Notifications in Xamarin.Android"
description: Learn about the UI elements of an Android notification and the APIs involved with creating and displaying a notification.
ms.service: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
---

# Notifications in Xamarin.Android

This section explains how to implement notifications in
Xamarin.Android. It describes the various UI elements of an Android
notification and discusses the API's involved with creating and
displaying a notification.

## [Local notifications in Android](local-notifications.md)

This section explains how to implement local notifications in
Xamarin.Android. It describes the various UI elements of an Android
notification and discuss the APIs involved with creating and
displaying a notification.

## [Walkthrough - using local notifications in Xamarin.Android](local-notifications-walkthrough.md)  

This walkthrough covers how to use local notifications in a 
Xamarin.Android application. It demonstrates the basics of creating and 
publishing a notification. When the user clicks on the notification in 
the notification drawer it starts up a second Activity. 

## Further reading

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
 &ndash; Firebase Cloud Messaging (FCM) is a service that facilitates
 messaging between mobile apps and server applications. Firebase Cloud
 Messaging can be used to implement remote notifications (also called
 push notifications) in Xamarin.Android applications.

[Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
 &ndash; This Android Developer topic is the definitive guide for
 Android notifications. It includes a design considerations section
 that helps you design your notifications so that they conform to the
 guidelines of the Android user interface. It provides more background
 information about preserving navigation when starting an Activity,
 and it explains how to display progress in a notification and control
 media playback on the Lock Screen.

[NotificationListenerService](xref:Android.Service.Notification.NotificationListenerService)
 &ndash; This Android service makes it possible for your app to listen
 to (and interact with) all notifications posted on the Android device,
 not just the notifications that your app is registered to receive.
 Note that the user must explicitly give permission to your app for it
 to be able to listen for notifications on the device.

## Related links

- [Local Notifications (sample)](/samples/xamarin/monodroid-samples/localnotifications)