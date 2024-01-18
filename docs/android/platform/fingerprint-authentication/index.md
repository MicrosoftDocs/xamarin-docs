---
title: "Fingerprint Authentication"
description: "This guide discusses how to add fingerprint authentication, introduced in Android 6.0, to a Xamarin.Android application."
ms.service: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Fingerprint Authentication

_This guide discusses how to add fingerprint authentication, introduced in Android 6.0, to a Xamarin.Android application._

## Fingerprint Authentication Overview

The arrival of fingerprint scanners on Android devices provides applications with an alternative to the traditional username/password method of user authentication. The use of fingerprints to authenticate a user makes it possible for an application to incorporate security that is less intrusive than a username and password.

The FingerprintManager APIs target devices with a fingerprint scanner and are running API level 23 (Android 6.0) or higher. The APIs are found in the `Android.Hardware.Fingerprints` namespace. The Android Support Library v4 provides versions of the fingerprint APIs meant for older versions of Android. The compatibility APIs are found in the `Android.Support.v4.Hardware.Fingerprint` namespace, are distributed through the [Xamarin.Android.Support.v4 NuGet package](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

The [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (and its Support Library counterpart, [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) is the primary class for using the fingerprint scanning hardware. This class is an Android SDK wrapper around the system level service that manages interactions with the hardware itself. It is responsible for starting the fingerprint scanner and for responding to feedback from the scanner. This class has a fairly straightforward interface with only three members:

- **`Authenticate`** &ndash; This method will initialize the hardware scanner and start the service in the background, waiting for the user to scan their fingerprint.
- **`EnrolledFingerprints`** &ndash; This property will return `true` if the user has registered one or more fingerprints with the device.
- **`HardwareDetected`** &ndash; This property is used to determine if the device supports fingerprint scanning.

The `FingerprintManager.Authenticate` method is used by an Android application to start the fingerprint scanner. The following snippet is an example of how to invoke it using the Support Library compatibility APIs:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

This guide will discuss how to use the `FingerprintManager` APIs to enhance an Android application with fingerprint authentication. It will cover how to instantiate and create a `CryptoObject` to help secure the results from the fingerprint scanner. We'll examine how an application should subclass `FingerprintManager.AuthenticationCallback` and respond to feedback from the fingerprint scanner. Finally, we'll see how to enroll a fingerprint on an Android device or emulator and how to use **adb** to simulate a fingerprint scan.

## Requirements

Fingerprint Authentication requires Android 6.0 (API level 23) or higher and a device with a fingerprint scanner. 

A fingerprint must already be enrolled with the device for each user that is to be authenticated. This involves setting up a screen lock that uses a password, PIN, swipe pattern, or facial recognition. It is possible to simulate some of the fingerprint authentication functionality in an Android Emulator.  For more information on these two topics, please see the [Enrolling a Fingerprint](enrolling-fingerprint.md) section. 

## Related Links

- [Fingerprint Guide Sample App](/samples/xamarin/monodroid-samples/fingerprintguide)
- [Fingerprint Dialog Sample](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Requesting Permissions at Runtime](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [Fingerprint and payments API (video)](https://youtu.be/VOn7VrTRlA4)