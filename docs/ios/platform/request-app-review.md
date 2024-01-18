---
title: "Request App Review in Xamarin.iOS"
description: "This article describes the RequestReview method that Apple added to iOS 10, and discusses how to implement it in Xamarin.iOS."
ms.service: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Request App Review in Xamarin.iOS

_This article covers the RequestReview method that Apple added to iOS 10 and how to implement it in Xamarin.iOS._

New to iOS 10.3, the `RequestReview()` method allows an iOS app to ask the user to rate or review it. When this method is called in a shipping app that the user has installed from the App Store, iOS 10 will handle the entire rating and review process for the developer. Because this process is governed by App Store policy, an alert may or may not be displayed.

![A sample Review Request alert](request-app-review-images/review01.png)

## Requesting a Rating or Review

While the `RequestReview()` static method of the `SKStoreReviewController` class can be called at any point where it makes sense in the user experience, the review process is governed and handled by App Store policy. As a result, this method may or may not display an alert and should never be called in response to a user action, such as tapping a button.

For example, an app might request a review after it has been launched a given number of times or a game might request a review after the player finishes a level.

To requests a review as soon as an Xamarin.iOS app finishes launching, make the following changes to the `AppDelegate.cs` file:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }

        ...

    }
}
```

> [!NOTE]
> Calling  `RequestReview()` in an under-development app will always display the rating and review dialog so it can be tested. This does not apply to apps that have been distributed through TestFlight, where the method call will be ignored.

When the `RequestReview()` method is called in a shipping app that the user has installed from the App Store, iOS 10 will handle the entire rating and review process for the developer. Again, because this process is governed by App Store policy, an alert may or may not be displayed.

## Linking to an App Store Product Page 

In addition to the new `RequestReview` method, the developer can still provide a deep link to the app's product page in the App Store from within an app. By appending `action=write-review` to the end of the product page URL, a page will be opened where the user can write a review of the app automatically. 

## Summary

This article has covered the RequestReview method that Apple added to iOS 10 and how to implement it in Xamarin.iOS.

## Related Links

- [iOSTenThree Sample](/samples/xamarin/ios-samples/ios10-iostenthree/)