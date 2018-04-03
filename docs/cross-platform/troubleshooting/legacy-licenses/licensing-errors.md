---
title: "Some specific licensing errors"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
---

# Some specific licensing errors

> [!IMPORTANT]
> The information below does not apply to MSDN users, because they are problems specific to legacy Xamarin licenses. If you are an MSDN user and you see errors similar to those below, please try [updating to the latest version of Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & refer to this [Licensing Options guide](~/cross-platform/get-started/requirements.md).



## "Invalid license"

> Invalid license. Please reactivate Xamarin.Android (XA9999)
> Unable to determine license edition. (XA9010)

These messages can appear in a few different circumstances.

-   The current license on disk might be out-of-sync with the current user information on the computer. [Refreshing the license files](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) will resolve this problem in many cases.

-   The computer might have 2 conflicting duplicate activations. This type of license is no longer used by Xamarin. If you experience issues that appear to be related to this error, please [Email Support](https://www.xamarin.com/support).

-   The Xamarin account might not yet be invited to a Xamarin license. See also: [Team License Management](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## "Failed to load Android Entitlements"

> Mono.VisualStudio.Shell.ShellPackage Error: 0 : Failed to load Android entitlements: Invalid license. Please reactivate Xamarin.Android (XA9999)
> Mono.VisualStudio.Shell.ShellPackage Error: 0 : Failed license update: Invalid license. Please reactivate Xamarin.Android (XA9999)

This is an older version of the "Invalid license" problem from above. The same troubleshooting steps apply.

## "This version was released after your subscription expired"

> Error XA9000: This version was released after your subscription expired (11/11/2014 5:11:41 PM). (XA9000) (Mobile.Android.Utilities)
> Error XA9010: Unable to determine license edition. (XA9010) (Mobile.Android.Utilities)

These messages commonly appear in two situations:

-   The on-disk licenses are outdated. [Refreshing the license files](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) will resolve this problem in many cases.

-   The Xamarin subscription is no longer active, and the current installed version of Xamarin is more recent than the expiration date of the subscription. In this case the correct fix is to [downgrade](http://kb.xamarin.com/customer/portal/articles/1699777) to a version of Xamarin that was released before the subscription expired. Feel free to send an email to [hello@xamarin.com](mailto:hello@xamarin.com) to request the latest valid version for your subscription. If you still see the error after downgrading, be sure to try refreshing the license files too.

## Additional references

-   [How to refresh licenses](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [How to downgrade Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [List of Xamarin.Android error codes](~/android/troubleshooting/errors.md)
-   [List of MonoTouch (Xamarin.iOS) error codes](~/ios/troubleshooting/mtouch-errors.md)

### Next Steps
For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed.
