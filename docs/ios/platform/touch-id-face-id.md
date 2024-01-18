---
title: "Touch ID and Face ID with Xamarin.iOS"
description: "This document provides a high-level description of biometric authentication in iOS."
ms.service: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.subservice: xamarin-ios
author: profexorgeek
ms.author: jusjohns
ms.date: 12/16/2019
no-loc: [Objective-C]
---

# Use Touch ID and Face ID with Xamarin.iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/ios-samples/ios11-faceidsample/)

iOS supports two biometric authentication systems:

1. **Touch ID** uses a fingerprint sensor under the Home button.
1. **Face ID** uses front-facing camera sensors to authenticate users with a facial scan.

Touch ID was introduced in iOS 7 and Face ID in iOS 11.

These authentication systems rely on a hardware-based security processor called the _Secure Enclave_. The Secure Enclave is responsible for encrypting mathematical representations of face and fingerprint data, and authenticating users using this information. According to Apple, face and fingerprint data do not leave the device and are not backed up to iCloud. Apps interact with the Secure Enclave through the _Local Authentication_ API and cannot retrieve face or fingerprint data or directly access the Secure Enclave.

Touch ID and Face ID can be used by apps to authenticate a user before providing access to protected content.

## Local authentication context

Biometric authentication on iOS relies on a _local authentication context_ object, which is an instance of the `LAContext` class. The `LAContext` class allows you to:

- Check the availability of biometric hardware.
- Evaluate authentication policies.
- Evaluate access controls.
- Customize and display authentication prompts.
- Reuse or invalidate an authentication state.
- Manage credentials.

## Detect available authentication methods

The sample project includes an `AuthenticationView` backed by an `AuthenticationViewController`. This class overrides the `ViewWillAppear` method to detect available authentication methods:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    public override void ViewWillAppear(bool animated)
    {
        base.ViewWillAppear(animated);
        unAuthenticatedLabel.Text = "";
    
        var context = new LAContext();
        var buttonText = "";

        // Is login with biometrics possible?
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out var authError1))
        {
            // has Touch ID or Face ID
            if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
            {
                context.LocalizedReason = "Authorize for access to secrets"; // iOS 11
                BiometryType = context.BiometryType == LABiometryType.TouchId ? "Touch ID" : "Face ID";
                buttonText = $"Login with {BiometryType}";
            }
            // No FaceID before iOS 11
            else
            {
                buttonText = $"Login with Touch ID";
            }
        }

        // Is pin login possible?
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out var authError2))
        {
            buttonText = $"Login"; // with device PIN
            BiometryType = "Device PIN";
        }

        // Local authentication not possible
        else
        {
            // Application might choose to implement a custom username/password
            buttonText = "Use unsecured";
            BiometryType = "none";
        }
        AuthenticateButton.SetTitle(buttonText, UIControlState.Normal);
    }
}
```

The `ViewWillAppear` method is called when the UI is about to display to the user. This method defines a new instance of `LAContext` and uses the `CanEvaluatePolicy` method to determine if biometric authentication is enabled. If so, it checks the system version and `BiometryType` enum to determine which biometric options are available.

If biometric authentication is not enabled, the app attempts to fall back to PIN authentication. If neither biometric nor PIN authentication is available, the device owner has not enabled security features and content cannot be secured through local authentication.

## Authenticate a user

The `AuthenticationViewController` in the sample project includes an `AuthenticateMe` method, which is responsible for authenticating the user:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    partial void AuthenticateMe(UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var localizedReason = new NSString("To access secrets");
    
        // Because LocalAuthentication APIs have been extended over time,
        // you must check iOS version before setting some properties
        context.LocalizedFallbackTitle = "Fallback";
    
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            context.LocalizedCancelTitle = "Cancel";
        }
        if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
        {
            context.LocalizedReason = "Authorize for access to secrets";
            BiometryType = context.BiometryType == LABiometryType.TouchId ? "TouchID" : "FaceID";
        }
    
        // Check if biometric authentication is possible
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = $"{BiometryType} Authentication Failed";
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, localizedReason, replyHandler);
        }

        // Fall back to PIN authentication
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = "Device PIN Authentication Failed";
                        AuthenticateButton.Hidden = true;
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, localizedReason, replyHandler);
        }

        // User hasn't configured any authentication: show dialog with options
        else
        {
            unAuthenticatedLabel.Text = "No device auth configured";
            var okCancelAlertController = UIAlertController.Create("No authentication", "This device does't have authentication configured.", UIAlertControllerStyle.Alert);
            okCancelAlertController.AddAction(UIAlertAction.Create("Use unsecured", UIAlertActionStyle.Default, alert => PerformSegue("AuthenticationSegue", this)));
            okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine("Cancel was clicked")));
            PresentViewController(okCancelAlertController, true, null);
        }
    } 
}
```

The `AuthenticateMe` method is called in response to the user tapping a **Login** button. A new `LAContext` object is instantiated and the device version is checked to determine which properties to set on the local authentication context.

The `CanEvaluatePolicy` method is called to check if biometric authentication is enabled, fall back to PIN authentication if possible, and finally offer an unsecured mode if no authentication is available. If an authentication method is available, the `EvaluatePolicy` method is used to show the UI and complete the authentication process.

The sample project contains mock data and a view to display the data if authentication is successful.

## Related links

- [Local Authentication using Touch ID or Face ID sample](/samples/xamarin/ios-samples/ios11-faceidsample/)
- [About Touch ID](https://support.apple.com/en-us/HT204587) on support.apple.com
- [About Face ID](https://support.apple.com/en-us/HT208108) on support.apple.com