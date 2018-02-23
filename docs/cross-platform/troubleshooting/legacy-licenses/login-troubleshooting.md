---
title: "Why can't I log into Xamarin in Visual Studio or Visual Studio for Mac?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
---

# Why can't I log into Xamarin in Visual Studio or Visual Studio for Mac?

> [!IMPORTANT]
> This guide does not apply to most MSDN users because they are not required to own or log into Xamarin accounts unless using the [Xamarin Components store](https://components.xamarin.com/) or [Visual Studio for Mac (Mac)](~/cross-platform/get-started/requirements.md). MSDN license holders should refer to this [Licensing Options guide](~/cross-platform/get-started/requirements.md) instead.



## Overview
There are a few common causes that may prevent you from logging into your Xamarin account in the IDE. Known issues & fixes are described below.

### Finding the Login Screen

For reference, the login screens are found here:

- Visual Studio for Mac
   - Top right corner of the welcome screen
   - **Visual Studio for Mac > Account** (Mac)
   - **Tools > Account** (Windows)
- Visual Studio
   - **Tools > Xamarin Account**

## The IDE is connecting, but the account screen isn't showing correct login information

This issue is typically resolved by a manual license resync.
Please follow the directions in this article: [How do I manually resyncronize Xamarin licenses?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## Invalid Account Information

If you go to the Xamarin website [login page](https://store.xamarin.com/Login?from=%2faccount%2f), you can try logging in with your current account credentials.
The page also has links to reset your password and to create a new account.

## Account is valid, but the IDE can't connect

This is usually caused when firewall or other security settings block the IDE from accessing needed endpoints.
The activation servers must access the following:

> activation.xamarin.com
> store.xamarin.com
> auth.xamarin.com

However, several other endpoints are important for general development processes, such as updates, getting NuGet packages, etc. Because of this, it's recommended to make sure *ALL* of the endpoints are added from the [Xamarin firewall configuration guide](~/cross-platform/get-started/installation/firewall.md).

### iOS in Xamarin Studio Windows
iOS developement is not supported in Xamarin Studio for Windows. When accessing the login screen here, the iOS license will not be displayed.

Instead, login via Xamarin Studio (Mac) or Visual Studio with a legacy license. Note that MSDN users on Windows do not need to log in.

## Additional Support

If the above scenarios do not describe your situation or resolve the issue, please refer to these [support options](https://www.xamarin.com/support).
