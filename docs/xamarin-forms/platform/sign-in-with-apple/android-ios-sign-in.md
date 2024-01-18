---
title: "Use Sign In with Apple for Xamarin.Forms"
description: "Learn how to implement Sign In with Apple in your Xamarin.Forms mobile applications."
ms.service: xamarin
ms.assetid: 2E47E7F2-93D4-4CA3-9E66-247466D25E4D
ms.subservice: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Use Sign In with Apple in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/signinwithapple/)

Sign In with Apple is for all new applications on iOS 13 that use third-party authentication services. The implementation details between iOS and Android are quite different. This guide walks through how you can do this today in Xamarin.Forms.

In this guide and sample, specific platform services are used to handle Sign In with Apple:

- Android using a generic web service talking to Azure Functions with OpenID/OpenAuth
- iOS uses the native API for authentication on iOS 13, and falls back to a generic web service for iOS 12 and below

## A sample Apple sign in flow

This sample offers an opinionated implementation for getting Apple Sign In to work in your Xamarin.Forms app.

We use two Azure Functions to help with the authentication flow:

1. `applesignin_auth` - Generates the Apple Sign In Authorization URL and redirects to it.  We do this on the server side, instead of the mobile app, so we can cache the `state` and validate it when Apple's servers send a callback.
2. `applesignin_callback` - Handles the POST callback from Apple and securely exchanges the authorization code for an Access Token and ID Token.  Finally, it redirects back to the App's URI Scheme, passing back the tokens in a URL Fragment.

The mobile app registers itself to handle the custom URI scheme we have selected (in this case `xamarinformsapplesignin://`) so the `applesignin_callback` function can relay the tokens back to it.

When the user starts authentication, the following steps happen:

1. The mobile app generates a `nonce` and `state` value and passes them to the `applesignin_auth` Azure function.
2. The `applesignin_auth` Azure function generates an Apple Sign In Authorization URL (using the provided `state` and `nonce`), and redirects the mobile app browser to it.
3. The user enters their credentials securely in the Apple Sign In authorization page hosted on Apple's servers.
4. After the Apple Sign In flow finishes on Apple's servers, Apple Redirects to the `redirect_uri` which will be the `applesignin_callback` Azure function.
5. The request from Apple sent to the `applesignin_callback` function is validated to ensure the correct `state` is returned, and that the ID Token claims are valid.
6. The `applesignin_callback` Azure function exchanges the `code` posted to it by Apple, for an _Access Token_, _Refresh Token_, and _ID Token_ (which contains claims about the User ID, Name, and Email).
7. The `applesignin_callback` Azure function finally redirects back to the app's URI scheme (`xamarinformsapplesignin://`) appending a URI fragment with the Tokens (e.g. `xamarinformsapplesignin://#access_token=...&refresh_token=...&id_token=...`).
8. The Mobile app parses out the URI Fragment into an `AppleAccount` and validates the `nonce` claim received matches the `nonce` generated at the start of the flow.
9. The mobile app is now authenticated!

## Azure Functions

This sample uses Azure Functions. Alternatively, an ASP.NET Core Controller or similar web server solution could deliver the same functionality.

### Configuration

Several app settings need to be configured when using Azure Functions:

- `APPLE_SIGNIN_KEY_ID` - This is your `KeyId` from earlier.
- `APPLE_SIGNIN_TEAM_ID` - This is usually your _Team ID_ found in your [Membership Profile](https://developer.apple.com/account/#/membership)
- `APPLE_SIGNIN_SERVER_ID`: This is the `ServerId` from earlier.  It's *not* your App _Bundle ID_, but rather the *Identifier* of the *Services ID* you created.
- `APPLE_SIGNIN_APP_CALLBACK_URI` - This is the custom URI Scheme you want to redirect back to your app with.  In this sample `xamarinformsapplesignin://` is used.
- `APPLE_SIGNIN_REDIRECT_URI` - The *Redirect URL* you setup when creating your *Services ID* in the *Apple Sign In* Configuration section.  To test, it might look something like: `http://local.test:7071/api/applesignin_callback`
- `APPLE_SIGNIN_P8_KEY` - The text contents of your `.p8` file, with all the `\n` newlines removed so it's one long string

### Security considerations

**Never** store your P8 key inside of your application code. Application code is easy to download and disassemble. 

It is also considered a bad practice to use a `WebView` to host the authentication flow, and to intercept URL Navigation events to obtain the authorization code. At this time there is currently no fully secure way to handle Sign In with Apple on non iOS13+ devices without hosting some code on a server to handle the token exchange. We recommend hosting the authorization url generation code on a server so you can cache the state and validate it when Apple issues a POST callback to your server.

## A cross-platform sign in service

Using the Xamarin.Forms DependencyService, you can create separate authentication services that use the platform services on iOS, and a generic web service for Android and other non-iOS platforms based on a shared interface.

```csharp
public interface IAppleSignInService
{
    bool Callback(string url);

    Task<AppleAccount> SignInAsync();
}
```

On iOS, the native APIs are used:

```csharp
public class AppleSignInServiceiOS : IAppleSignInService
{
#if __IOS__13
    AuthManager authManager;
#endif

    bool Is13 => UIDevice.CurrentDevice.CheckSystemVersion(13, 0);
    WebAppleSignInService webSignInService;

    public AppleSignInServiceiOS()
    {
        if (!Is13)
            webSignInService = new WebAppleSignInService();
    }

    public async Task<AppleAccount> SignInAsync()
    {
        // Fallback to web for older iOS versions
        if (!Is13)
            return await webSignInService.SignInAsync();

        AppleAccount appleAccount = default;

#if __IOS__13
        var provider = new ASAuthorizationAppleIdProvider();
        var req = provider.CreateRequest();

        authManager = new AuthManager(UIApplication.SharedApplication.KeyWindow);

        req.RequestedScopes = new[] { ASAuthorizationScope.FullName, ASAuthorizationScope.Email };
        var controller = new ASAuthorizationController(new[] { req });

        controller.Delegate = authManager;
        controller.PresentationContextProvider = authManager;

        controller.PerformRequests();

        var creds = await authManager.Credentials;

        if (creds == null)
            return null;

        appleAccount = new AppleAccount();
        appleAccount.IdToken = JwtToken.Decode(new NSString(creds.IdentityToken, NSStringEncoding.UTF8).ToString());
        appleAccount.Email = creds.Email;
        appleAccount.UserId = creds.User;
        appleAccount.Name = NSPersonNameComponentsFormatter.GetLocalizedString(creds.FullName, NSPersonNameComponentsFormatterStyle.Default, NSPersonNameComponentsFormatterOptions.Phonetic);
        appleAccount.RealUserStatus = creds.RealUserStatus.ToString();
#endif

        return appleAccount;
    }

    public bool Callback(string url) => true;
}

#if __IOS__13
class AuthManager : NSObject, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding
{
    public Task<ASAuthorizationAppleIdCredential> Credentials
        => tcsCredential?.Task;

    TaskCompletionSource<ASAuthorizationAppleIdCredential> tcsCredential;

    UIWindow presentingAnchor;

    public AuthManager(UIWindow presentingWindow)
    {
        tcsCredential = new TaskCompletionSource<ASAuthorizationAppleIdCredential>();
        presentingAnchor = presentingWindow;
    }

    public UIWindow GetPresentationAnchor(ASAuthorizationController controller)
        => presentingAnchor;

    [Export("authorizationController:didCompleteWithAuthorization:")]
    public void DidComplete(ASAuthorizationController controller, ASAuthorization authorization)
    {
        var creds = authorization.GetCredential<ASAuthorizationAppleIdCredential>();
        tcsCredential?.TrySetResult(creds);
    }

    [Export("authorizationController:didCompleteWithError:")]
    public void DidComplete(ASAuthorizationController controller, NSError error)
        => tcsCredential?.TrySetException(new Exception(error.LocalizedDescription));
}
#endif
```

The compile flag `__IOS__13` is used to provide support for iOS 13 as well as legacy versions that fallback to the generic web service.

On Android, the generic web service with Azure Functions is used:

```csharp
public class WebAppleSignInService : IAppleSignInService
{
    // IMPORTANT: This is what you register each native platform's url handler to be
    public const string CallbackUriScheme = "xamarinformsapplesignin";
    public const string InitialAuthUrl = "http://local.test:7071/api/applesignin_auth";

    string currentState;
    string currentNonce;

    TaskCompletionSource<AppleAccount> tcsAccount = null;

    public bool Callback(string url)
    {
        // Only handle the url with our callback uri scheme
        if (!url.StartsWith(CallbackUriScheme + "://"))
            return false;

        // Ensure we have a task waiting
        if (tcsAccount != null && !tcsAccount.Task.IsCompleted)
        {
            try
            {
                // Parse the account from the url the app opened with
                var account = AppleAccount.FromUrl(url);

                // IMPORTANT: Validate the nonce returned is the same as our originating request!!
                if (!account.IdToken.Nonce.Equals(currentNonce))
                    tcsAccount.TrySetException(new InvalidOperationException("Invalid or non-matching nonce returned"));

                // Set our account result
                tcsAccount.TrySetResult(account);
            }
            catch (Exception ex)
            {
                tcsAccount.TrySetException(ex);
            }
        }

        tcsAccount.TrySetResult(null);
        return false;
    }

    public async Task<AppleAccount> SignInAsync()
    {
        tcsAccount = new TaskCompletionSource<AppleAccount>();

        // Generate state and nonce which the server will use to initial the auth
        // with Apple.  The nonce should flow all the way back to us when our function
        // redirects to our app
        currentState = Util.GenerateState();
        currentNonce = Util.GenerateNonce();

        // Start the auth request on our function (which will redirect to apple)
        // inside a browser (either SFSafariViewController, Chrome Custom Tabs, or native browser)
        await Xamarin.Essentials.Browser.OpenAsync($"{InitialAuthUrl}?&state={currentState}&nonce={currentNonce}",
            Xamarin.Essentials.BrowserLaunchMode.SystemPreferred);

        return await tcsAccount.Task;
    }
}
```

## Summary

This article described the steps necessary to setup Sign In with Apple for use in your Xamarin.Forms applications.

## Related links

- [XamarinFormsAppleSignIn (Sample)](https://github.com/Redth/Xamarin.AppleSignIn.Sample)
- [Sign In with Apple Guidelines](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)