---
title: "Scanning For Fingerprints"
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
---

# Scanning For Fingerprints

Now that we have seen how to prepare a Xamarin.Android application to use fingerprint authentication, let's return to the  `FingerprintManager.Authenticate` method, and discuss its place in the Android 6.0 fingerprint authentication. A quick overview of the workflow for fingerprint authentication is described in this list:

1. Invoke `FingerprintManager.Authenticate`, passing a `CryptoObject` and a `FingerprintManager.AuthenticationCallback` instance. The `CryptoObject` is used to ensure that the fingerprint authentication result was not tampered with. 
2. Subclass the [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) class. An instance of this class will be provided to `FingerprintManager` when fingerprint authentication starts. When the fingerprint scanner is finished, it will invoke one of the callback methods on this class.
3. Write code to update the UI to let the user know that the device has started the fingerprint scanner and is waiting for user interaction. 
4. When the fingerprint scanner is done, Android will return results to the application by invoking a method on the `FingerprintManager.AuthenticationCallback` instance that was provided in the previous step.
5. The application will inform the user of the fingerprint authentication results and react to the results as appropriate. 

The following code snippet is an example of a method in an Activity that will start scanning for fingerprints:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Let's discuss each of these parameters in the `Authenticate` method in a bit more detail:

* The first parameter is a _crypto_ object that the fingerprint scanner will use to help authenticate the results of a fingerprint scan. This object may be `null`, in which case the application has to blindly trust that nothing has tampered with the fingerprint results. It is recommended that a `CryptoObject` be instantiated and provided to the `FingerprintManager` rather than null. [Creating a CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) will explain in detail how to instantiate a `CryptoObject` based on a `Cipher`.
* The second parameter is always zero. The Android documentation identifies this as set of flags and is most likely reserved for future use. 
* The third parameter, `cancellationSignal` is an object used to turn off the fingerprint scanner and cancel the current request. This is an [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html), and not a type from the .NET framework.
* The fourth parameter is mandatory and is a class that subclasses the `AuthenticationCallback` abstract class. Methods on this class will be invoked to signal to clients when the `FingerprintManager` has finished and what the results are. As there is a lot to understand about implementing the `AuthenticationCallback`, it will be covered in [it's own section](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* The fifth parameter is an optional `Handler` instance. If a `Handler` object is provided, the `FingerprintManager` will use the `Looper` from that object when processing the messages from the fingerprint hardware. Typically, one does not need to provide a `Handler`, the FingerprintManager will use the `Looper` from the application.

## Cancelling a Fingerprint Scan

It might be necessary for the user (or the application) to cancel the fingerprint scan after it has been initiated. In this situation, invoke the [`IsCancelled`](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) method on the [`CancellationSignal`](http://developer.android.com/reference/android/os/CancellationSignal.html) that was provided to `FingerprintManager.Authenticate` when it was invoked to start the fingerprint scan.

Now that we have seen the `Authenticate` method, let's examine some of the more important parameters in more detail. First, we'll look at [Responding to Authentication Callbacks](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), which will discuss how to subclass the [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), enabling an Android application to react to the results provided by the fingerprint scanner.




## Related Links

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
