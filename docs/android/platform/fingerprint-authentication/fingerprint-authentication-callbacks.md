---
title: "Responding to Authentication Callbacks"
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
---

# Responding to Authentication Callbacks

The fingerprint scanner runs in the background on its own thread, and
when it is finished it will report the results of the scan by invoking
one method of `FingerprintManager.AuthenticationCallback` on the UI
thread. An Android application must provide its own handler which
extends this abstract class, implementing all the following methods:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Called when there is an unrecoverable error. There is nothing more an application or user can do to correct the situation except possibly try again.
* **`OnAuthenticationFailed()`** &ndash; This method is invoked when a fingerprint has been detected but not recognized by the device.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Called when there is a recoverable error, such as the finger being swiped to fast over the scanner.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; This is called when a fingerprint has been recognized.

If a `CryptoObject` was used when calling `Authenticate`, it is
recommended to call `Cipher.DoFinal` in `OnAuthenticationSuccessful`.
`DoFinal` will throw an exception if the cipher was tampered with or
improperly initialized, indicating that the result of the fingerprint
scanner may have been tampered with outside of the application.


> [!NOTE]
> It is recommended to keep the callback class relatively light weight and free of application specific logic. The callbacks should act as a "traffic cop" between the Android application and the results from the fingerprint scanner.

## A Sample Authentication Callback Handler

The following class is an example of a minimal `FingerprintManager.AuthenticationCallback` implementation: 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` checks to see if a `Cipher` was provided to `FingerprintManager` when `Authentication` was invoked. If so, the `DoFinal` method is called on the cipher. This closes the `Cipher`, restoring it to its original state. If there was a problem with the cipher, then `DoFinal` will throw an exception and the authentication attempt should be considered to have failed.

The `OnAuthenticationError` and `OnAuthenticationHelp` callbacks each receive an integer indicating what the problem was. The following section explains each of the possible help or error codes. The two callbacks serve similar purposes &ndash; to inform the application that fingerprint authentication has failed. How they differ is in severity. `OnAuthenticationHelp` is a user recoverable error, such as swiping the fingerprint too fast; `OnAuthenticationError` is more a severe error, such as a damaged fingerprint scanner.

Note that `OnAuthenticationError` will be invoked when the fingerprint scan is cancelled via the `CancellationSignal.Cancel()` message. The `errMsgId` parameter will have the value of 5 (`FingerprintState.ErrorCanceled`). Depending on the requirements, an implementation of the `AuthenticationCallbacks` may treat this situation differently than the other errors. 

`OnAuthenticationFailed` is invoked when the fingerprint was successfully scanned but did not match any fingerprint enrolled with the device. 

## Help Codes and Error Message Ids 

A list and description of the error codes and help codes may be found in the [Android SDK documentation](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) for the FingerprintManager class. Xamarin.Android represents these values with the `Android.Hardware.Fingerprints.FingerprintState` enum:


-   **`AcquiredGood`** &ndash; (value 0) The image acquired was good.


-   **`AcquiredImagerDirty`** &ndash; (value 3) The fingerprint image was too noisy due to suspected or detected dirt on the sensor. For example, it's reasonable to return this after multiple `AcquiredInsufficient` or actual detection of dirt on the sensor (stuck pixels, swaths, etc.). The user is expected to take action to clean the sensor when this is returned.


-   **`AcquiredInsufficient`** &ndash; (value 2) The fingerprint image was too noisy to process due to a detected condition (i.e. dry skin) or a possibly dirty sensor (See `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (value 1) Only a partial fingerprint image was detected. During enrollment, the user should be informed on what needs to happen to resolve this problem, e.g., &ldquo;press firmly on sensor.&rdquo;



-   **`AcquiredTooFast`** &ndash; (value 5) The fingerprint image was incomplete due to quick motion. While mostly appropriate for linear array sensors, this could also happen if the finger was moved during acquisition. The user should be asked to move the finger slower (linear) or leave the finger on the sensor longer.




-   **`AcquiredToSlow`** &ndash; (value 4) The fingerprint image was unreadable due to lack of motion. This is most appropriate for linear array sensors that require a swipe motion.



-   **`ErrorCanceled`** &ndash; (value 5) The operation was canceled because the fingerprint sensor is unavailable. For example, this may happen when the user is switched, the device is locked, or another pending operation prevents or disables it.



-   **`ErrorHwUnavailable`** &ndash; (value 1) The hardware is unavailable. Try again later.




-   **`ErrorLockout`** &ndash; (value 7) The operation was canceled because the API is locked out due to too many attempts.




-   **`ErrorNoSpace`** &ndash; (value 4) Error state returned for operations like enrollment; the operation cannot be completed because there's not enough storage remaining to complete the operation.



-   **`ErrorTimeout`** &ndash; (value 3) Error state returned when the current request has been running too long. This is intended to prevent programs from waiting for the fingerprint sensor indefinitely. The timeout is platform and sensor-specific, but is generally about 30 seconds.



-   **`ErrorUnableToProcess`** &ndash; (value 2) Error state returned when the sensor was unable to process the current image.



## Related Links

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
