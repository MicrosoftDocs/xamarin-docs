---
title: "Authenticating Users with Azure Active Directory B2C"
description: "Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications. This article demonstrates how to use Microsoft Authentication Library and Azure Active Directory B2C to integrate consumer identity management into a mobile application."
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
---

# Authenticating Users with Azure Active Directory B2C

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications. This article demonstrates how to use Microsoft Authentication Library and Azure Active Directory B2C to integrate consumer identity management into a mobile application._

![](~/media/shared/preview.png "This API is currently pre-release")

> [!NOTE]
> The [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) is still in preview, but is suitable for use in a production environment. However, there may be breaking changes to the API, internal cache format, and other mechanisms of the library, which may impact your application.

## Overview

Azure Active Directory B2C is an identity management service for consumer-facing applications, that allows consumers to sign-in to your application by:

- Using their existing social accounts (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Creating new credentials (email address and password, or username and password). These credentials are referred to as *local* accounts.

The process for integrating the Azure Active Directory B2C identity management service into a mobile application is as follows:

1. Create an Azure Active Directory B2C tenant. For more information, see [Create an Azure Active Directory B2C tenant in the Azure portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Register your mobile application with the Azure Active Directory B2C tenant. The registration process assigns an **Application ID** that uniquely identifies your application, and a **Redirect URL** that can be used to direct responses back to your application. For more information, see [Azure Active Directory B2C: Register your application](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Create a sign-up and sign-in policy. This policy will define the experiences that consumers will go through during sign-up and sign-in, and also specifies the contents of tokens the application will receive on successful sign-up or sign-in. For more information, see [Azure Active Directory B2C: Built-in policies](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Use the [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) in your mobile application to initiate an authentication workflow with your Azure Active Directory B2C tenant.

> [!NOTE]
> As well as integrating Azure Active Directory B2C identity management into mobile applications, MSAL can also be used to integrate Azure Active Directory identity management into mobile applications. This can be accomplished by registering a mobile application with Azure Active Directory at the [Application Registration Portal](https://apps.dev.microsoft.com/). The registration process assigns an **Application ID** that uniquely identifies your application, which should be specified when using MSAL. For more information, see [How to register an app with the v2.0 endpoint](/azure/active-directory/develop/active-directory-v2-app-registration/), and [Authenticate Your Mobile Apps Using Microsoft Authentication Library](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) on the Xamarin blog.

MSAL uses the device's web browser to perform authentication. This improves the usability of an application, as users only need to sign-in once per device, improving conversion rates of sign-in and authorization flows in the application. The device browser also provides improved security. After the user completes the authentication process, control will return to the application from the web browser tab. This is achieved by registering a custom URL scheme for the redirect URL that's returned from the authentication process, and then detecting and handling the custom URL once it's sent. For more information about choosing a custom URL scheme, see [Choosing a native app redirect URI](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> The mechanism for registering a custom URL scheme with the operating system and handling the scheme is specific to each platform.

Each request that is sent to an Azure Active Directory B2C tenant specifies a *policy*. Policies describe consumer identity experiences such as sign-up, or sign-in. For example, a sign-up policy allows the behavior of the Azure Active Directory B2C tenant to be configured through the following settings:

- Account types that consumers can use to sign-in to the application.
- Data to be collected from the consumer during sign-up.
- Multi-factor authentication.
- Sign-up page content.
- Token claims that the mobile application receives when the policy has executed.

An Azure Active Directory tenant can contain multiple policies of different types, which can then be used in your application as required. In addition, policies can be reused across applications, allowing you to define and modify consumer identity experiences without changing your code. For more information about policies, see [Azure Active Directory B2C: Built-in policies](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## Setup

The Microsoft Authentication Library (MSAL) NuGet library must be added to the Portable Class Library (PCL) project and platform projects in a Xamarin.Forms solution. The following sections provide additional setup instructions for using MSAL to communicate with an Azure Active Directory B2C tenant from a mobile application.

### Portable Class Library

PCLs that consume MSAL will need to be retargeted to use Profile7. For more information about PCLs, see [Introduction to Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).

### iOS

On iOS, the custom URL scheme that was registered with Azure Active Directory B2C must be registered in **Info.plist**, as shown in the following screenshot:

![](azure-ad-b2c-images/customurl-ios.png "Registering a Custom URL Scheme on iOS")

When Azure Active Directory B2C completes the authorization request, it redirects to the registered redirect URL. Because the URL uses a custom scheme it results in iOS launching the mobile application, passing in the URL as a launch parameter, where it's processed by the `OpenUrl` override of the application's `AppDelegate` class, which is shown in the following code example:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

The code in the `OpenURL` method ensures that control returns to MSAL once the interactive portion of the authentication workflow has ended.

### Android

On Android, the custom URL scheme that was registered with Azure Active Directory B2C must be registered in **AndroidManifest.xml**, by adding an `<activity>` element inside the existing `<application>` element. The `<activity>` element specifies the `IntentFilter` on the `Activity` that handles the scheme, and is shown in the following example:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

When Azure Active Directory B2C completes the authorization request, it redirects to the registered redirect URL. Because the URL uses a custom scheme it results in Android launching the mobile application, passing in the URL as a launch parameter, where it's processed by the `microsoft.identity.client.BrowserTabActivity`. Note that the `data android:scheme` property must be set to the custom URL scheme that's registered with the Azure Active Directory B2C application.

In addition, the `MainActivity` class must be modified, as shown in the following code example:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

The `OnCreate` method is modified by assigning a `UIParent` instance to the `App.UiParent` property. This ensures that the authentication flow occurs in the context of the current activity.

The code in the `OnActivityResult` method ensures that control returns to MSAL once the interactive portion of the authentication workflow has ended.

### Universal Windows Platform

On the Universal Windows Platform, no additional setup is required to use MSAL.

## Initialization

The Microsoft Authentication Library uses members of the `PublicClientApplication` class to initiate an authentication workflow. The sample application declares and initializes a `public` property of this type, named `ADB2CClient`, in the `AuthenticationProvider` class. The following code example shows how this property is initialized:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

When the mobile application was registered with the Azure Active Directory B2C tenant, the registration process assigned an **Application ID**. This ID must be specified in the `PublicClientApplication` constructor, along with an `Authority` constant that comprises a base URL, and the Azure Active Directory B2C policy to be executed.

## Signing In

The sign-in screen in the sample application is shown in the following screenshots:

![](azure-ad-b2c-images/login.png "Login Page")

Sign-in with social identity providers, or with a local account, are permitted. While Microsoft, Google, and Facebook, as shown above, are used as social identity providers, other identity providers can also be used.

The following code example shows how the sign-in process is invoked:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

The `AcquireTokenAsync` method launches the device's web browser and displays the authentication options defined in the Azure Active Directory B2C policy that's specified by the policy referenced through the `Constants.Authority` constant. This policy defines the experiences that consumers will go through during sign-up and sign-in, and the claims the application will receive on successful sign-up or sign-in.

The result of the `AcquireTokenAsync` method call is an `AuthenticationResult` instance. If authentication is successful, the `AuthenticationResult` instance will contain an identity token, which will be cached locally. If authentication is unsuccessful, the `AuthenticationResult` instance will contain data that indicates why authentication failed.

In the sample application, if authentication is successful, the `TodoList` page is navigated to.

## Silent Re-authentication

When the `LoginPage` in the sample application appears, an attempt is made to retrieve a user token without showing any authentication user interface. This is achieved with the `AcquireTokenSilentAsync` method, as demonstrated in the following code example:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

The `AcquireTokenSilentAsync` method attempts to retrieve a user token from the cache, without requiring the user to sign-in. This handles the scenario where a suitable token may already be present in the cache from previous sessions. If the attempt to obtain a token is successful, the `TodoList` page is navigated to. If the attempt to obtain a token is unsuccessful, nothing happens and the user will have the choice to initiate a new authentication workflow.

## Signing Out

The following code example shows how the sign-out process is invoked:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

This clears all the authentication tokens from the local cache.

## Summary

This article demonstrated how to use Microsoft Authentication Library (MSAL) and Azure Active Directory B2C to integrate consumer identity management into a mobile application. Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.


## Related Links

- [AzureADB2CAuth (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
