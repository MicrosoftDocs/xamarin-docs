---
title: "Web Views in Xamarin.iOS"
description: "This document describes the various ways a Xamarin.iOS app can display web content. It discusses UIWebView, WKWebView, SFSafariViewController, Safari, and app transport security."
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
---

# Web Views in Xamarin.iOS

Over the lifetime of iOS Apple has released a number of ways for app developers to incorporate web view functionality in their apps. Most users utilize the built-in Safari web browser on their iOS device, and therefore expect that web-view functionality from other apps is consistent with this experience. They expect the same gestures to work, the performance to be on par, and the functionality the same.

In this article, we will explore each of the three Web Views provided by Apple: `UIWebView`, `WKWebview`, and `SFSafariViewController`, their similarities and differences, and how they can be used. 

iOS 11 introduced new changes to both `WKWebView` and `SFSafariViewController`. For more information on these, see the [Web changes in iOS 11 guide](~/ios/platform/introduction-to-ios11/web.md) guide.

## UIWebView

`UIWebView` is Apple's legacy way of providing web content in your app. It was released in iOS 2.0, and has been deprecated as of 8.0.

If you plan to support iOS versions earlier than 8.0, you will have to use `UIWebView`. Due to the fact that `UIWebView` is less optimized for performance than the alternatives, it is recommended that you should check the user's iOS version. If it 8.0 or above, using either of the options explain below will create a better user experience.
 
To add a UIWebView to your Xamarin.iOS app, use the following code:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

This produces the following web view:

[![](uiwebview-images/webview.png "The effect of ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

For more information on using `UIWebView`, refer to the following recipes:


- [Load a Web Page](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Load Local Content](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Load Non-Web Documents](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## WKWebView

`WKWebView` was introduced in iOS 8 allowing app developers to implement a web browsing interface similar to that of mobile Safari. This is due, in part, to the fact that `WKWebView` uses the Nitro Javascript engine, the same engine used by mobile Safari. `WKWebView` should always be used over UIWebView were possible due to the [increased performance](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview),built in user-friendly gestures, and the ease of interaction between the web page and your app.
  
`WKWebView` can be added to your app in an almost identical way to UIWebView, however as the developer you have much more control over the UI/UX and functionality. Creating and displaying the web view object will display the requested page, however you can control how the view is presented, how the user can navigate, and how the user exits the view.  

The code below can be used to launch a `WKWebView` in your Xamarin.iOS app:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

This produces the following web view:

[![](uiwebview-images/wkwebview.png "An example web view without ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

It is important to note that `WKWebView` is in the WebKit namespace, so you will have to add this using directive to the top of your class.

`WKWebView` can also be used within Xamarin.Mac apps, and you therefore may want to consider using it if you are creating a cross-platform Mac/iOS app.

The [Handle JavaScript Alerts](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) recipe also provides information on using WKWebView with Javascript

<a name="safariviewcontroller" />

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

[![](uiwebview-images/sfsafariviewcontroller.png "An example web view with SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## Safari

It is also possible to open the mobile Safari app from within your app, by using the code below:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

This produces the following web view:

[![](uiwebview-images/safari.png "A web page presented in Safari")](uiwebview-images/safari.png#lightbox)

Navigating users away from your app to Safari should generally always be avoided. Most users will not expect navigation outside of your application, so if you navigate away from your app, users may never return it, essentially killing engagement.

iOS 9 improvements allow the user to easily return to your app through a back button that is provided in the top left corner of the Safari page.

## App Transport Security

App Transport Security, or *ATS* was introduced by Apple in iOS 9 to ensure that all internet communications conform to secure connection best practices.

For more information on ATS, including how to implement it in your app, refer to the [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

## Related Links

- [WebViews (sample)](https://developer.xamarin.com/samples/monotouch/WebView/)
