---
title: "Web Views in Xamarin.iOS"
description: "This document describes the various ways a Xamarin.iOS app can display web content. It discusses WKWebView, SFSafariViewController, Safari, and app transport security."
ms.service: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
no-loc: [Objective-C]
---

# Web Views in Xamarin.iOS

Over the lifetime of iOS Apple has released a number of ways for app developers to incorporate web view functionality in their apps. Most users utilize the built-in Safari web browser on their iOS device, and therefore expect that web-view functionality from other apps is consistent with this experience. They expect the same gestures to work, the performance to be on par, and the functionality the same.

iOS 11 introduced new changes to both `WKWebView` and `SFSafariViewController`. For more information on these, see the [Web changes in iOS 11 guide](~/ios/platform/introduction-to-ios11/web.md) guide.

## WKWebView

`WKWebView` was introduced in iOS 8 allowing app developers to implement a web browsing interface similar to that of mobile Safari. This is due, in part, to the fact that `WKWebView` uses the Nitro Javascript engine, the same engine used by mobile Safari. `WKWebView` should always be used over UIWebView where possible due to the increased performance, built in user-friendly gestures, and the ease of interaction between the web page and your app.

`WKWebView` can be added to your app in an almost identical way to UIWebView, however as the developer you have much more control over the UI/UX and functionality. Creating and displaying the web view object will display the requested page, however you can control how the view is presented, how the user can navigate, and how the user exits the view.  

The code below can be used to launch a `WKWebView` in your Xamarin.iOS app:

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://learn.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

It is important to note that `WKWebView` is in the `WebKit` namespace, so you will have to add this using directive to the top of your class.

`WKWebView` can also be used in Xamarin.Mac apps, and you should use it if you are creating a cross-platform Mac/iOS app.

The [Handle JavaScript Alerts](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) recipe also provides information on using WKWebView with Javascript.

## SFSafariViewController

 `SFSafariViewController` is the latest way to provide web content from your app and is available in iOS 9 and later. Unlike `UIWebView` or `WKWebView`, `SFSafariViewController` is a View Controller and so cannot be used with other views. You should present `SFSafariViewController` as a new View Controller, in the same way you would present any View Controller.

 `SFSafariViewController` is essentially a 'mini safari' that can be embedded into your app. Like WKWebView it uses the same Nitro Javascript Engine, but also provides a range of additional Safari features such as AutoFill, Reader, and the ability to share cookies and data with mobile Safari. Interaction between the user and the `SFSafariViewController` is not accessible to your app. Your app will not have access to any of the default Safari features.

It also, by default, implements a **Done** button, allowing to user to easily return to your app, and forward and back navigation buttons, allowing your user to navigate through a stack of web pages. In addition, it also provides the user with an address bar giving them the peace of mind that they are on the expected web page. The address bar does not allow the user to change the url.

These implementations cannot be changed, so `SFSafariViewController` is ideal to use as the default browser if your app wants to present a webpage without any customization.

The code below can be used to launch a `SFSafariViewController` in your Xamarin.iOS app:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

This produces the following web view:

[![An example web view with SFSafariViewController](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## Safari

It is also possible to open the mobile Safari app from within your app, by using the code below:

```csharp
var url = new NSUrl("https://learn.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

This produces the following web view:

[![A web page presented in Safari](webview-images/safari.png)](webview-images/safari.png#lightbox)

Navigating users away from your app to Safari should generally always be avoided. Most users will not expect navigation outside of your application, so if you navigate away from your app, users may never return it, essentially killing engagement.

iOS 9 improvements allow the user to easily return to your app through a back button that is provided in the top left corner of the Safari page.

## App Transport Security

App Transport Security, or *ATS* was introduced by Apple in iOS 9 to ensure that all internet communications conform to secure connection best practices.

For more information on ATS, including how to implement it in your app, refer to the [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

## UIWebView deprecation

`UIWebView` is Apple's legacy way of providing web content in your app. It was released in iOS 2.0, and has been deprecated as of 8.0.

> [!IMPORTANT]
> `UIWebView` is deprecated. New apps using this control will [not be accepted into the
> App Store as of April 2020, and apps updates using this control will not be accepted by December 2020](https://developer.apple.com/news/?id=12232019b).
>
> [Apple's `UIWebView` documentation](https://developer.apple.com/documentation/uikit/uiwebview) suggests apps should use [`WKWebView`](#wkwebview) instead.
>
> If you are looking for resources in regard to the `UIWebView` deprecation warning (ITMS-90809) while using Xamarin.Forms, please refer to the [Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) documentation.

Developers who submitted iOS applications in the last six months (or so) might have received a warning from the App Store, about `UIWebView` being deprecated.

Deprecations of APIs are common. Xamarin.iOS uses custom attributes to signal those APIs (and suggest replacements when available) back to the developers. What is different this time, and much less common, is that the deprecation **will be enforced** by Apple's App Store at submission time.

Unfortunately, removing the `UIWebView` type from `Xamarin.iOS.dll` is a [binary breaking change](/dotnet/core/compatibility/categories#binary-compatibility). This change will break existing 3rd party libraries, including some that might not be supported or even re-compilable anymore (for example, closed source). This will only create additional problems for developers. Therefore, we are not removing the type *yet*.

Starting with [Xamarin.iOS 13.16](/xamarin/ios/release-notes/13/13.16) new detection and tools are available to help you migrate from `UIWebView`.

### Detection

If you have'nt recently submitted an iOS application to the Apple App Store, you might wonder if this situation applies to your application(s).

To find out, you can add `--warn-on-type-ref=UIKit.UIWebView` to your project's **Additional mtouch arguments.** This will warn of **any** reference to the deprecated `UIWebView` inside your application (and all its dependencies). Different warnings are used to report types **before** and **after** the managed linker has executed.

The warnings, like others, can be turned into errors using `-warnaserror:`. This can be useful if you want to ensure a new dependency to `UIWebView` is not added after your verifications. For example:

* `-warnaserror:1502` will report errors if any references are found in pre-linked assemblies.
* `-warnaserror:1503` will report errors if any references are found in post-linked assemblies.

You can also silence the warnings if either the pre/post linking results are not useful. For example:

* `-nowarn:1502` will **not** report warnings if any references are found in pre-linked assemblies.
* `-nowarn:1503` will **not** report warnings if any references are found in post-linked assemblies.

### Removal

Every application is unique. Removing `UIWebView` from your application can require different steps depending on how and where it is used. The most common scenarios are as follows:

- There is no use of `UIWebView` inside your application. Everything is fine. You should **not** have warnings when submitting to the AppStore. Nothing else is required from you.
- Direct usage of `UIWebView` by your application. Start by removing your use of `UIWebView`, for example, replace it with the newer `WKWebView` (iOS 8) or `SFSafariViewController` (iOS 9) types. Once this is completed the managed linker should not see any reference to `UIWebView` and the final app binary will have no trace of it.
- Indirect usage. `UIWebView` can be present in some third party libraries, either managed or native that is used by your application. Start by updating your external dependencies to their latest versions since this situation might already be solved in a newer release. If not, contact the maintainer(s) of the libraries and ask about their update plans.

Alternatively, you could try the following approaches:

1. If you're using **Xamarin.Forms**, then read this [blog post](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/).
1. Enable the managed linker (on the whole project or, at least, on the dependency using `UIWebView`) so it *might* be removed, if not referenced. That will solve the problem but might required additional work to make your code linker-safe.
1. If you are unable to change the managed linker settings, see the special cases below.

#### Applications cannot use the linker (or change its settings)

If for some reason you are **not** using the managed linker (for example, **Don't link**) then the `UIWebView` symbol will remain in the binary app you submit it to Apple and it might be rejected.

A *forceful* solution is to add `--optimize=force-rejected-types-removal` to your project's **Additional mtouch arguments**. This will remove traces of `UIWebView` from the application. However, any code that refers to the type will **not** work properly (expect exceptions or crashes). This approach should be used only if you're sure that the code is not reachable at runtime (even if it was reachable through static analysis).

#### Support for iOS 7.x (or earlier)

`UIWebView` has been part of iOS since v2.0. The most common replacements are `WKWebView` (iOS 8) and `SFSafariViewController` (iOS 9). If your application still supports older iOS versions you should consider the following options:

* Make iOS 8 your minimum target version (a build time decision).
* Only use `WKWebView` if the app is running on iOS 8+ (a runtime decision).

#### Applications not submitted to Apple

If your application isn't submitted to Apple, you should plan to move away from the deprecated API since it can be removed in future iOS releases. However, you can do this transition using your own timetable.

## Related links

- [WebViews (sample)](/samples/xamarin/ios-samples/webview)
