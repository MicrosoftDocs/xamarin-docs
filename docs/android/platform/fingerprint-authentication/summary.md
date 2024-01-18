---
title: "Fingerprint Authentication Guidance"
description: Review the guidance for using fingerprint authentication allowing a Xamarin.Android application to verify users.
ms.service: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Fingerprint Authentication Guidance

## Fingerprint Authentication Guidance

Now that we have seen the concepts and APIs surrounding Android 6.0 fingerprint authentication, let's discuss some general advice for the use of the Fingerprint APIs.

1. **Use the Android Support Library v4 Compatibility APIs** &ndash; This will simplify the application code by removing the API check from the code and allow an application to target the most devices possible.
2. **Provide Alternatives to Fingerprint Authentication** &ndash; Fingerprint authentication is a great, quick way for an application to authenticate a user, however, it cannot be assumed that it will always work or be available. It is possible that the fingerprint scanner may fail, the lens maybe be dirty, the user may not have configured the device to use fingerprint authentication, or the fingerprints have since gone missing. It is also possible that the user may not wish to use fingerprint authentication with your application. For these reasons, an Android application should provide an alternate authentication process such as username and password.
3. **Use Google's fingerprint icon** &ndash; All applications should use the same fingerprint icon provided by Google. The use of a standard icon makes it easy for Android users to recognize where in apps fingerprint authentication is used: 
    
    ![Android fingerprint icon](summary-images/ic-fp-40px.png)
    
4. **Notify the User** &ndash; An application should display some kind of notification to the user that the fingerprint scanner is active and awaiting a touch or swipe. 

## Summary

Fingerprint authentication is a great way to allow a Xamarin.Android application to quickly verify users, making it easier for users to interact with sensitive features such as in-app purchases. This guide discussed the concepts and code that is required to incorporate the Android 6.0 fingerprint API's in your Xamarin.Android application.

First we discussed the fingerprint API's themselves, `FingerprintManager` (and `FingerprintManagerCompat`). We examined how the `FingerprintManager.AuthenticationCallbacks` abstract class must be extended by an application and used as an intermediary between the fingerprint hardware and the application itself. Then we examined how to verify the integrity of the fingerprint scanner results using a Java `Cipher` object. Finally, we touched a bit on testing by describing how to enroll a fingerprint on a device and using **adb** to simulate a fingerprint swipe on an emulator. 

If you haven't already done so, you should look at the [sample application](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) that accompanies this guide. The [Fingerprint Dialog Sample](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) has been ported from Java to Xamarin.Android and provides another example on how to add fingerprint authentication to an Android application.

## Related Links

- [Fingerprint Guide Sample App](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Fingerprint Dialog Sample](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Fingerprint Icon](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)